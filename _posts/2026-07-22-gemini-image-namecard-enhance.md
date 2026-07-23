---
layout: post
title: "[GCP 實戰] LINE 名片 Bot 再進化：用 Gemini 一次搞定名片正反面辨識合併"
description: "紀錄如何為 LINE 名片助理機器人加上正反面辨識功能，讓 Gemini 在單次呼叫中同時看到名片正反兩面圖片，自動合併中英文對照欄位，並解決背後的記憶體狀態機設計與資源清理問題。"
category:
- AI
- GCP
- Python
tags: ["Google Cloud", "Vertex AI", "Gemini API", "LINE Bot", "Python", "Multimodal"]
---

# 痛點：一張中文名片，其實是兩張名片

台灣的名片有個很常見的設計：正面印中文，背面印英文（或反過來）。我們的 LINE 名片 Bot 原本的邏輯很單純：收到一張圖片，OCR 一次，存一筆資料。

問題就出在這裡。使用者傳正面，Bot 存了一筆只有中文姓名的資料；如果使用者接著把背面也傳過去，Bot 會把它當成「另一張新名片」，同一個人在資料庫裡就會出現兩筆各缺一半資訊的紀錄。使用者得自己手動比對、刪除重複資料，體驗非常糟。

這篇文章記錄我們如何讓 Bot 學會「這是同一張名片的兩面」，並且把正反面資訊合併成一筆完整資料。

---

# 解法：先問一句「還有背面嗎？」

比起自己寫規則去猜兩張圖片是不是同一張名片，我們選了一個更直接的做法：**問使用者**。

流程設計如下：

1. 使用者傳送名片正面，Bot 照常 OCR。
2. OCR 完成後，Bot **不馬上存檔**，而是回覆一句「📇 已辨識正面資料，這張名片還有背面嗎？」，並附上兩顆 Quick Reply 按鈕。
3. 使用者按「沒有，直接儲存」→ 照原本流程存檔，結束。
4. 使用者按「有背面」→ Bot 記住剛剛那張正面圖片，等待下一張圖片進來。
5. 背面圖片一到，兩張圖片一起送進 Gemini，合併成一筆資料再存檔。

這個等待狀態我們用專案原本就有的 `user_states` 記憶體字典來管理，並且加上 5 分鐘的逾時：使用者按了「有背面」卻不理它、跑去做別的事，5 分鐘一過就當作放棄，不會卡住整個流程。

```python
user_states[user_id] = {
    'action': 'pending_backside_confirm',
    'card_obj': card_obj,
    'front_image_bytes': image_content,
    'expires_at': time.time() + PENDING_BACKSIDE_TIMEOUT_SECONDS
}
```

---

# 核心：讓 Gemini 一次看兩張圖，自己做合併

最關鍵的技術決定是：要不要分兩次辨識正面、背面，再自己寫程式合併？我們選擇了另一條路，把正反面圖片包在同一次 `generate_content` 請求裡，直接交給 Gemini 判斷。

原因很簡單：中英文姓名要合併成「王大明 David Wang」這種格式，靠字串規則去兜很容易兜得又醜又不準；但這種語意層級的整合，靠規則去湊反而更容易出錯，交給 Gemini 直接判斷比較省事。

在 [app/gemini_utils.py](file:///Users/al03034132/Documents/linebot-namecard-python/app/gemini_utils.py) 中新增了 `generate_json_from_two_images`，沿用既有的 `NAMECARD_SCHEMA` 結構化輸出，只是這次 `contents` 塞了兩個圖片 Part：

```python
def generate_json_from_two_images(
        front_img: PIL.Image.Image,
        back_img: PIL.Image.Image,
        prompt: str) -> object:
    model = GenerativeModel(
        "gemini-3-flash-preview",
        generation_config={
            "response_mime_type": "application/json",
            "response_schema": NAMECARD_SCHEMA
        },
    )
    front_part = Part.from_data(
        data=pil_to_bytes(front_img), mime_type="image/jpeg")
    back_part = Part.from_data(
        data=pil_to_bytes(back_img), mime_type="image/jpeg")
    response = model.generate_content(
        [prompt, front_part, back_part],
        stream=False,
        labels={"client_id": "namecard"}
    )
    return response
```

Prompt 也只是在原本的 `IMGAGE_PROMPT` 後面多加一段合併指示：

```python
DOUBLE_SIDED_IMAGE_PROMPT = IMGAGE_PROMPT + """
這兩張圖片是同一張名片的正面與背面，請整合成一筆完整資料。
若同一欄位中英文都有出現（如姓名、公司），請合併呈現
（例如「王大明 David Wang」）；
若某欄位只有一面出現，直接採用該面的值；忽略明顯重複的資訊。
"""
```

一次 API 呼叫，換來的是完全不用自己寫合併規則、也不用維護那套規則。

---

# 容易忽略的兩個小坑

功能上線前的整體 code review，抓到兩個很容易被忽略、但真的會咬人的細節。

### 坑一：重複檢查的時機點

原本的重複檢查（比對 email 是否已存在）是 OCR 完馬上做。但雙面辨識上線後，如果正面剛好跟舊資料的 email 重複，這時候就提早判定「已存在」並結束流程，那背面圖片裡如果帶有新的 email，就再也沒有機會被看到了。

修法是把重複檢查往後挪，統一放到「單面選擇不合併」或「雙面合併完成」之後才做，確保永遠是拿最終版本的資料去比對：

```python
async def _finalize_and_save_card(card_obj, event, user_id) -> None:
    existing_card_id = firebase_utils.check_if_card_exists(card_obj, user_id)
    if existing_card_id:
        # ... 回覆已存在
        return
    card_id = firebase_utils.add_namecard(card_obj, user_id)
    # ... 回覆儲存成功
```

單面流程、雙面合併流程最後都收斂呼叫這個共用函式，重複檢查只會在資料底定的那一刻執行一次。

### 坑二：清狀態不能一路清到底

`user_states` 這個字典其實同時被好幾個功能共用：編輯備忘錄、修改欄位、這次的背面辨識都會往裡面寫東西。一開始的實作圖方便，只要偵測到殘留狀態就整個刪掉再處理新事件。

問題是：如果使用者正在「編輯電話欄位」等你輸入新號碼，這時候手滑傳了一張圖片，這段邏輯會把 `editing_field` 的狀態也一起清掉，使用者原本的編輯操作就這樣被默默取消了。

修法是只清跟背面辨識流程相關的兩個狀態，其他狀態一律不碰：

```python
if state.get('action') in (
    'pending_backside_confirm', 'awaiting_backside_image'
):
    del user_states[user_id]
```

同樣的邏輯後來也發現漏掉了一個分支，處理「操作已過期」回覆的地方一開始也是整個清掉，統一改成一樣的選擇性判斷後才算真正補齊。

---

# 順手做的資源清理：別讓背面圖片一直留在記憶體裡

`awaiting_backside_image` 狀態裡存的不只是文字，還有正面圖片的原始位元組資料。如果使用者問完「還有背面嗎」就人間蒸發，這包資料理論上會一直留在 process 記憶體裡，因為原本的設計只有「使用者下次互動」才會順便檢查並清掉逾時狀態。

我們加了一個 `sweep_expired_states()`，掛在 Webhook 進來的第一時間執行一次，把所有使用者裡已經過期的暫存狀態清掉，不用等到當事人自己回來才被動清理：

```python
def sweep_expired_states() -> None:
    now = time.time()
    expired_user_ids = [
        user_id for user_id, state in user_states.items()
        if 'expires_at' in state and state['expires_at'] <= now
    ]
    for user_id in expired_user_ids:
        del user_states[user_id]
```

只要任何一個使用者傳訊息進來，就會順便幫全體使用者做一次垃圾清理，讓放棄流程的人不會留下永久佔用記憶體的殘骸。

---

# 總結與效益

這次的雙面辨識合併功能，讓 LINE 名片 Bot 更貼近台灣使用者真實的名片使用習慣：

1. **一次辨識，資料完整**：正反兩面一次送進 Gemini，中英文欄位自動合併，不用再手動比對重複資料。
2. **不打斷使用者**：忽略提示、逾時、臨時去做別的事，都能自然退回單面儲存，不會卡住流程。
3. **重複檢查對準最終資料**：確保比對的永遠是合併後的完整版本，不會漏接背面才出現的新資訊。
4. **狀態機互不干擾**：背面辨識的暫存狀態只影響自己，不會誤傷使用者正在進行的其他操作。
5. **記憶體用得乾淨**：主動清掃逾時狀態，放棄流程的使用者不會留下看不見的記憶體負擔。

完整程式碼已同步推播至 [GitHub](https://github.com/kkdai/linebot-namecard-python)，歡迎參考！
