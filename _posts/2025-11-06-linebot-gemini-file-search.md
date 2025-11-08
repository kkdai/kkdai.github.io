---
layout: post
title: "[Python] 用 Python + Gemini File Search 打造智能文件助手 LINE Bot：讓 AI 幫你讀文件"
description: "使用 Python 結合 LINE Bot SDK、Google Gemini File Search API 和 FastAPI，打造一個能理解文件內容、回答問題的智能文件助手"
category:
- TIL
tags: ["Python", "LINE Bot", "GCP", "Gemini", "FastAPI", "Cloud Run", "File Search"]

---

<img src="../images/image-20251108080734448.png" alt="image-20251108080734448" style="zoom:50%;" />

# 前情提要

在工作和生活中，我們經常需要處理大量的文件：會議記錄、技術文件、合約、研究報告等等。每次要找特定資訊時，都得翻開文件一頁一頁找，既費時又容易遺漏重點。

最近 Google 推出了 [Gemini File Search API](https://ai.google.dev/gemini-api/docs/file-search?hl=zh-tw)，讓 AI 可以直接分析上傳的文件並回答問題。我想到，如果能結合 LINE Bot，讓大家透過最常用的通訊軟體就能「問」文件問題，那不是很方便嗎？

想像一下這些場景：
- 📄 **會議記錄**：「這次會議的主要決議是什麼？」
- 📊 **技術文件**：「這個 API 的參數有哪些？」
- 🖼️ **圖片內容**：「這張圖片裡有什麼？」
- 📑 **研究報告**：「這份報告的結論是什麼？」

於是我決定動手打造這個「智能文件助手 LINE Bot」，讓 AI 成為你的私人文件分析師！

### 專案程式碼

[https://github.com/kkdai/linebot-file-search-adk](https://github.com/kkdai/linebot-file-search-adk)

（透過這個程式碼，可以快速部署到 GCP Cloud Run，享受無伺服器的便利）

<img src="../images/LINE 2025-11-08 08.07.11.tiff" alt="LINE 2025-11-08 08.07.11" style="zoom:50%;" />

##  📚 專案功能介紹




## 📚 專案功能介紹

### 核心功能

1. **📤 多格式檔案上傳**
   - 支援文件檔案：PDF、Word (DOCX)、純文字 (TXT) 等
   - 支援圖片檔案：JPG、PNG 等（利用 Gemini Image Understanding 圖片內容）
   - 自動處理中文檔名，避免編碼問題
   - 即時回饋上傳狀態

2. **🤖 AI 智能問答**
   - 基於 Google Gemini 2.5 Flash 模型
   - 從上傳的文件中搜尋相關內容並回答
   - 支援繁體中文、英文等多語言
   - 理解上下文，提供精準回答

3. **👥 多對話隔離**
   - **1對1聊天**：每個人有獨立的文件庫（完全隔離）
   - **群組聊天**：群組成員共享文件庫（協作查詢）
   - 自動識別對話類型，無需手動設定
   - File Search Store 自動建立和管理

4. **🔄 智能錯誤處理**
   - 檔案上傳失敗自動重試
   - 沒有文件時引導使用者上傳
   - 詳細的錯誤日誌記錄


## 💻 核心功能實作

### 1. File Search Store 的自動管理

這是整個系統的核心，負責管理每個使用者或群組的文件庫。

#### Store 命名策略

根據對話類型自動生成唯一的 store 名稱：

```python
def get_store_name(event: MessageEvent) -> str:
    """
    Get the file search store name based on the message source.
    Returns user_id for 1-on-1 chat, group_id for group chat.
    """
    if event.source.type == "user":
        return f"user_{event.source.user_id}"
    elif event.source.type == "group":
        return f"group_{event.source.group_id}"
    elif event.source.type == "room":
        return f"room_{event.source.room_id}"
    else:
        return f"unknown_{event.source.user_id}"
```

#### Store 存在性檢查與建立

關鍵的設計是：File Search Store 的 name 是由 API 自動生成的（例如 `fileSearchStores/abc123`），我們只能設定 `display_name`。因此需要透過 `list()` 和 `display_name` 來查找：

```python
async def ensure_file_search_store_exists(store_name: str) -> tuple[bool, str]:
    """
    Ensure file search store exists, create if not.
    Returns (success, actual_store_name).
    """
    try:
        # List all stores and check if one with our display_name exists
        stores = client.file_search_stores.list()
        for store in stores:
            if hasattr(store, 'display_name') and store.display_name == store_name:
                print(f"File search store '{store_name}' already exists: {store.name}")
                return True, store.name

        # Store doesn't exist, create it
        print(f"Creating file search store with display_name '{store_name}'...")
        store = client.file_search_stores.create(
            config={'display_name': store_name}
        )
        print(f"File search store created: {store.name} (display_name: {store_name})")
        return True, store.name

    except Exception as e:
        print(f"Error ensuring file search store exists: {e}")
        return False, ""
```

#### Cache 機制優化

為了避免每次都要 list 所有 stores，我們加入了快取機制：

```python
# Cache to store display_name -> actual_name mapping
store_name_cache = {}

# 在上傳時使用 cache
if store_name in store_name_cache:
    actual_store_name = store_name_cache[store_name]
else:
    success, actual_store_name = await ensure_file_search_store_exists(store_name)
    store_name_cache[store_name] = actual_store_name
```

### 2. 中文檔名的編碼問題處理

#### 問題分析

當檔案名稱包含中文字元時，直接傳給 API 會遇到 ASCII 編碼錯誤：

```
Error: 'ascii' codec can't encode characters in position 19-21: ordinal not in range(128)
```

#### 解決方案：檔案名稱安全化

我們採用「本地使用 ASCII 檔名，顯示時使用原始檔名」的策略：

```python
async def download_line_content(message_id: str, file_name: str) -> Optional[Path]:
    """
    Download file content from LINE and save to local uploads directory.
    """
    try:
        message_content = await line_bot_api.get_message_content(message_id)

        # Extract file extension from original file name
        _, ext = os.path.splitext(file_name)
        # Use safe file name (ASCII only) to avoid encoding issues
        safe_file_name = f"{message_id}{ext}"
        file_path = UPLOAD_DIR / safe_file_name

        async with aiofiles.open(file_path, 'wb') as f:
            async for chunk in message_content.iter_content():
                await f.write(chunk)

        print(f"Downloaded file: {file_path} (original: {file_name})")
        return file_path
    except Exception as e:
        print(f"Error downloading file: {e}")
        return None
```

這樣做的好處：
- ✅ 本地檔案路徑全為 ASCII（避免編碼問題）
- ✅ 使用者仍然看到原始中文檔名
- ✅ 支援任何語言的檔案名稱

### 3. 檔案上傳與狀態管理

完整的檔案上傳流程，包含等待 API 處理完成：

```python
async def upload_to_file_search_store(file_path: Path, store_name: str, display_name: Optional[str] = None) -> bool:
    """
    Upload a file to Gemini file search store.
    Returns True if successful, False otherwise.
    """
    try:
        # Check cache first
        if store_name in store_name_cache:
            actual_store_name = store_name_cache[store_name]
        else:
            success, actual_store_name = await ensure_file_search_store_exists(store_name)
            if not success:
                return False
            store_name_cache[store_name] = actual_store_name

        # Upload to file search store
        config_dict = {}
        if display_name:
            config_dict['display_name'] = display_name

        operation = client.file_search_stores.upload_to_file_search_store(
            file_search_store_name=actual_store_name,
            file=str(file_path),
            config=config_dict if config_dict else None
        )

        # Wait for operation to complete (with timeout)
        max_wait = 60  # seconds
        elapsed = 0
        while not operation.done and elapsed < max_wait:
            await asyncio.sleep(2)
            operation = client.operations.get(operation)
            elapsed += 2

        if operation.done:
            print(f"File uploaded successfully")
            return True
        else:
            print(f"Upload operation timeout")
            return False

    except Exception as e:
        print(f"Error uploading to file search store: {e}")
        return False
```

### 4. 智能查詢與 File Search 整合

當使用者提問時，系統會先檢查是否有上傳文件，然後使用 File Search 查詢：

```python
async def query_file_search(query: str, store_name: str) -> str:
    """
    Query the file search store using generate_content.
    Returns the AI response text.
    """
    try:
        # Get actual store name from cache or by searching
        actual_store_name = None

        if store_name in store_name_cache:
            actual_store_name = store_name_cache[store_name]
        else:
            # Try to find the store by display_name
            stores = client.file_search_stores.list()
            for store in stores:
                if hasattr(store, 'display_name') and store.display_name == store_name:
                    actual_store_name = store.name
                    store_name_cache[store_name] = actual_store_name
                    break

        if not actual_store_name:
            # Store doesn't exist - guide user to upload files
            return "📁 您還沒有上傳任何檔案。\n\n請先傳送文件檔案（PDF、DOCX、TXT 等）或圖片給我，上傳完成後就可以開始提問了！"

        # Create FileSearch tool with actual store name
        tool = types.Tool(
            file_search=types.FileSearch(
                file_search_store_names=[actual_store_name]
            )
        )

        # Generate content with file search
        response = client.models.generate_content(
            model=MODEL_NAME,
            contents=query,
            config=types.GenerateContentConfig(
                tools=[tool],
                temperature=0.7,
            )
        )

        if response.text:
            return response.text
        else:
            return "抱歉，我無法從文件中找到相關資訊。"

    except Exception as e:
        print(f"Error querying file search: {e}")
        return f"查詢時發生錯誤：{str(e)}"
```

### 5. LINE Bot Webhook 處理

FastAPI 的 webhook 處理，支援文字、檔案、圖片三種訊息類型：

```python
@app.post("/")
async def handle_callback(request: Request):
    signature = request.headers["X-Line-Signature"]
    body = await request.body()
    body = body.decode()

    try:
        events = parser.parse(body, signature)
    except InvalidSignatureError:
        raise HTTPException(status_code=400, detail="Invalid signature")

    for event in events:
        if not isinstance(event, MessageEvent):
            continue

        if event.message.type == "text":
            # Process text message
            await handle_text_message(event, event.message)
        elif event.message.type == "file":
            # Process file message
            await handle_file_message(event, event.message)
        elif event.message.type == "image":
            # Process image message
            await handle_file_message(event, event.message)

    return "OK"
```

## 🔧 遇到的挑戰與解決方案

### 1. File Search Store API 的名稱設計

**問題**：一開始以為可以直接指定 store 的 name，但實際上 `create()` 不接受 `name` 參數。

**錯誤訊息**：
```
FileSearchStores.create() got an unexpected keyword argument 'name'
```

**原因分析**：
- File Search Store 的 `name` 是由 API 自動生成的（格式：`fileSearchStores/xxxxx`）
- 我們只能設定 `display_name` 作為識別
- 需要透過 `list()` 遍歷來查找對應的 store

**解決方案**：
1. 使用 `display_name` 儲存我們定義的名稱（如 `user_U123456`）
2. 透過 `list()` 找到對應的 store 並取得實際的 `name`
3. 建立 cache 避免重複查詢

### 2. 中文檔名的編碼問題

**問題**：當檔案名稱包含中文時，API 呼叫會失敗。

**錯誤訊息**：
```
'ascii' codec can't encode characters in position 19-21: ordinal not in range(128)
```

**問題分析**：
```python
# 問題代碼：檔案路徑包含中文
file_path = "uploads/123456_會議記錄.pdf"  # ❌ 編碼錯誤
```

**解決方案**：
```python
# 解決方案：使用 ASCII 檔名，保留原始名稱供顯示
_, ext = os.path.splitext("會議記錄.pdf")
safe_file_name = f"{message_id}{ext}"  # "123456.pdf" ✅
file_path = UPLOAD_DIR / safe_file_name

# 在 config 中保留原始檔名
config = {'display_name': '會議記錄.pdf'}  # 用於 AI 回答時的引用
```

**好處**：
- 檔案系統操作使用 ASCII 路徑（不會出錯）
- AI 回答時仍然顯示原始中文檔名（使用者友善）

### 3. Store 不存在時的 404 錯誤

**問題**：首次上傳檔案時，store 還不存在就嘗試上傳。

**錯誤訊息**：
```
404 Not Found. {'message': '', 'status': 'Not Found'}
```

**解決方案**：
在上傳前先檢查並建立 store：

```python
# 1. 檢查 cache
if store_name in store_name_cache:
    actual_store_name = store_name_cache[store_name]
else:
    # 2. 檢查是否存在，不存在則建立
    success, actual_store_name = await ensure_file_search_store_exists(store_name)
    # 3. 加入 cache
    store_name_cache[store_name] = actual_store_name

# 4. 使用實際的 store name 上傳
operation = client.file_search_stores.upload_to_file_search_store(
    file_search_store_name=actual_store_name,
    file=str(file_path),
    config=config_dict
)
```

### 4. 非同步檔案處理

**問題**：上傳檔案是耗時操作，需要等待處理完成。

**解決方案**：
1. 使用 `aiofiles` 進行異步檔案讀寫
2. 使用 `asyncio.sleep()` 而非 `time.sleep()`
3. 實作輪詢機制等待操作完成

```python
# 等待上傳完成
max_wait = 60  # seconds
elapsed = 0
while not operation.done and elapsed < max_wait:
    await asyncio.sleep(2)  # 異步等待
    operation = client.operations.get(operation)
    elapsed += 2
```

### 5. VertexAI 不支援 File Search API

**問題**：原本想支援 VertexAI，但發現 File Search API 只支援 Gemini API。

**官方說明**：
根據 [Google AI 文件](https://ai.google.dev/gemini-api/docs/file-search?hl=zh-tw)，File Search 功能目前只支援透過 Gemini API 使用。

**解決方案**：
- 移除所有 VertexAI 相關程式碼和設定
- 簡化環境變數配置
- 只需要 `GOOGLE_API_KEY` 即可

## 📊 部署與維運

### 本地開發設定

```bash
# 1. 安裝依賴
pip install -r requirements.txt

# 2. 設定環境變數
export ChannelSecret="你的 LINE Channel Secret"
export ChannelAccessToken="你的 LINE Channel Access Token"
export GOOGLE_API_KEY="你的 Google Gemini API Key"

# 3. 啟動服務
uvicorn main:app --reload
```

### Docker 部署

```bash
# 建立映像
docker build -t linebot-file-search .

# 啟動容器
docker run -p 8000:8000 \
  -e ChannelSecret=你的SECRET \
  -e ChannelAccessToken=你的TOKEN \
  -e GOOGLE_API_KEY=你的API_KEY \
  linebot-file-search
```

### Google Cloud Run 部署

```bash
# 1. 建立並推送映像
gcloud builds submit --tag gcr.io/你的專案ID/linebot-file-search

# 2. 部署到 Cloud Run
gcloud run deploy linebot-file-search \
  --image gcr.io/你的專案ID/linebot-file-search \
  --platform managed \
  --region asia-east1 \
  --allow-unauthenticated \
  --set-env-vars ChannelSecret=你的SECRET,ChannelAccessToken=你的TOKEN,GOOGLE_API_KEY=你的API_KEY

# 3. 取得服務網址
gcloud run services describe linebot-file-search \
  --platform managed \
  --region asia-east1 \
  --format 'value(status.url)'
```

## 🎯 總結與未來改進

### 專案亮點

1. **開箱即用的文件助手**：無需安裝 APP，透過 LINE 就能使用
2. **智能文件分析**：結合 Gemini 2.5 Flash 的強大能力
3. **中文友善**：完整支援中文檔名和查詢
4. **隔離機制**：每個對話有獨立的文件庫，安全可靠
5. **自動化管理**：File Search Store 自動建立，使用者無感知

### 實戰經驗分享

在開發過程中，我深刻體會到：

#### 1. API 設計的差異

不同雲端服務的 API 設計理念差異很大：
- **Google Gemini**：name 由系統生成，開發者設定 display_name
- **需要適應**：透過 list + 遍歷來查找資源

這提醒我們：**閱讀官方文件比猜測 API 行為更重要**。

#### 2. 編碼問題無所不在

即使在 2024 年，編碼問題仍然存在：
- 檔案系統可能不支援 Unicode
- API 可能對特殊字元有限制
- **解決方案**：分離「儲存用檔名」和「顯示用檔名」

#### 3. 非同步程式設計的重要性

在處理外部 API 時：
- 使用 `async/await` 避免阻塞
- 使用 `asyncio.sleep()` 而非 `time.sleep()`
- 適當的 timeout 設定避免無限等待

### 未來改進方向

1. **效能優化**
   - 實作更完整的 cache 機制
   - 批次處理多檔案上傳
   - 壓縮大型檔案

2. **功能擴展**
   - 支援檔案刪除功能
   - 支援列出已上傳檔案
   - 支援檔案摘要生成
   - 整合更多 Gemini 功能（如圖片理解）

3. **使用體驗優化**
   - Rich Menu 設計
   - 更友善的錯誤提示
   - 上傳進度顯示
   - 查詢歷史記錄

4. **安全性強化**
   - 檔案大小限制
   - 檔案類型驗證
   - 使用者配額管理
   - 敏感資料過濾

### 關鍵學習

透過這個專案，我學到了：

1. **Google Gemini File Search** 的正確使用方式
2. **FastAPI** 在處理 LINE Bot webhook 的高效性
3. **Python async/await** 在 I/O 密集型應用的重要性
4. **編碼問題**的處理策略
5. **雲端原生**應用的設計模式

最重要的是：**AI 不只是聊天機器人，更是強大的內容分析工具**。File Search API 讓我們能輕鬆打造專業級的文件問答系統。

希望這個經驗分享能幫助到正在探索 AI 應用開發的朋友們！

### 相關資源

- [專案 GitHub Repository](https://github.com/kkdai/linebot-file-search-adk)
- [LINE Bot SDK for Python](https://github.com/line/line-bot-sdk-python)
- [Google Gemini File Search API](https://ai.google.dev/gemini-api/docs/file-search?hl=zh-tw)
- [FastAPI 文件](https://fastapi.tiangolo.com/)
- [Google Cloud Run 文件](https://cloud.google.com/run/docs)

---

**如果你覺得這個專案有幫助，歡迎給個 Star ⭐，或是分享給需要的朋友！**
