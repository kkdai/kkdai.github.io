---
layout: post
title: "[AI 實戰] 用 Gemini 3.5 Transcribe 做跟讀評分：讓 Song Lingo 聽你念歌詞"
description: "Google 在 8/26 發布了 Gemini 3.5 Transcribe，專門做語音轉文字，有逐字時間戳、85 種以上語言、講者分辨和自訂詞彙。我把它接進 Song Lingo，做成跟讀評分：念一句歌詞，App 標出哪個詞念對、念成別的詞、還是沒念到。這篇記錄模型本身、為什麼選它而不是 Live 版或一般的 Gemini、為什麼要用 verbatim 而不是官方主打的 smart 模式，以及幾個文件沒寫的地方：output_text 又是 SDK 自己組的欄位、詞的位置是用 UTF-8 位元組算的、按鈕在麥克風還沒準備好時就說「錄音中」，還有 Mac 上的無頭 Chrome 根本拿不到麥克風。"
category:
- AI
- Claude Code
tags: ["Gemini", "Gemini 3.5 Transcribe", "Speech-to-Text", "Language Learning", "Next.js", "Claude Code"]
---

# 前情提要

[第一篇](https://www.evanlin.com/gemini-song-lingo/)我用 Gemini 3.8 Flash TTS 做了 Song Lingo：貼 YouTube MV 網址，Gemini 轉錄歌詞、加上拼音翻譯文法，再由一位用 voice design 設計的老師逐句示範發音。[第二篇](https://www.evanlin.com/gemini-song-lingo-gcp-deploy/)把它部署到 Cloud Run，用 IAP 鎖到只有我自己能用。

部署完我在 GitHub 開了一份 roadmap，排了三件事：手機版面、跟讀評分、單字卡。手機版面已經做完了，這篇講的是第二件。

老師會示範，但我一直不知道自己念得對不對。**聽十遍老師念，不如自己念一遍、被指出哪裡錯。**這是 Song Lingo 從「聽歌看歌詞」變成「練發音」的關鍵一步。

---

# Gemini 3.5 Transcribe 是什麼

Google 在 8/26 發布了 [Gemini 3.5 Transcribe](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-5-transcribe/)，是兩個專門做語音轉文字的模型：

| 模型 | 用途 |
|---|---|
| `gemini-3.5-transcribe` | 錄好的音檔，整段送出轉錄 |
| `gemini-3.5-transcribe-live` | 透過 Live API 的 WebSocket 即時串流，一邊講一邊出字 |

幾個重點功能：

- **85 種以上語言**，會自動偵測，也能處理講到一半換語言的情況。
- **逐字時間戳**：每個詞都有開始和結束時間（整段轉錄的版本才有）。
- **講者分辨**：標出每句話是誰講的。
- **自訂詞彙**：最多 1,000 個專有名詞，提高特殊用語的辨識率。
- **兩種模式**：`smart` 會處理說話時的自我修正、拿掉語助詞、自動排版；`verbatim` 則是逐字照實轉錄。

官方部落格引用 Artificial Analysis 的測量：整段轉錄的平均字錯誤率（WER）2.6%，串流版 4.0%；和前一代 Chirp 3 比，拿到最終轉錄結果的時間快了 70%。價格沒有公布。

實際呼叫的方式和 TTS 一樣走 `interactions` API，但音檔要先用 Files API 上傳：

```python
uploaded = client.files.upload(file=audio_path, config={"mime_type": "audio/m4a"})
interaction = client.interactions.create(
    model="gemini-3.5-transcribe",
    input=[{"type": "audio", "uri": uploaded.uri, "mime_type": uploaded.mime_type}],
    generation_config={
        "transcription_config": {
            "language_codes": ["ja-JP"],
            "mode": {"type": "verbatim", "timestamp_granularities": ["word"]},
        }
    },
)
```

支援的格式很多，其中 `audio/webm` 剛好是 Chrome 錄音的格式，`audio/m4a` 則對應 iPhone Safari 錄出來的 AAC。

---

# 選哪個模型

做跟讀評分，其實有三個候選：

| 模型 | 適合 | 用在跟讀 |
|---|---|---|
| **`gemini-3.5-transcribe`** | 錄完整段再轉錄 | ✅ 有逐字時間戳，實測約 5 秒回來 |
| `gemini-3.5-transcribe-live` | 即時串流 | 一句歌詞只有幾秒，等念完再轉錄就夠了；要在 Cloud Run 和 IAP 後面維持 WebSocket 也麻煩得多 |
| `gemini-3.8-flash` 這類多模態模型 | 直接聽錄音給文字講評 | 結果每次不一定一樣，也沒有保證的逐字時間戳，不適合當評分的主體 |

最後選了 `gemini-3.5-transcribe`。專門的語音轉文字模型，結果穩定，有逐字資訊，而且每次只用大約 40 個 token，**不占 TTS 那每天 100 次的額度**。

---

# 先看真實回應，再寫程式

第一篇最貴的一課是：TTS 的網頁版照著 Python SDK 的寫法讀 `output_audio`，結果 REST 回應裡根本沒有這個欄位，一個小時燒光一整天的額度。

所以這次在 roadmap 的 issue 裡直接寫了一條規則：**先用真實的 API 回應驗證解析邏輯，再上線。**

測試音檔用 macOS 內建的 `say` 產生，內容是我自己寫的句子，一段念「今日は晴れです」，一段故意念成「今日は雨です」：

```bash
say -v Kyoko -o ok.aiff "今日は晴れです"
afconvert -f m4af -d aac ok.aiff ok.m4a   # 和 iPhone 錄音一樣是 AAC
```

然後攔截 SDK 送出與收到的原始 HTTP 內容，把回應的結構印出來。結果發現了兩件文件沒講清楚、照著寫就會出錯的事。

## `output_text` 又是 SDK 自己組的

文件說轉錄結果在 `interaction.output_text`。實際的 REST 回應長這樣：

```
steps[] → { type: "model_output",
            content[] → { type: "text", text: "今日は晴れです。",
                          annotations[] → { type: "word_info", text: "今日",
                                            start_index: 0, end_index: 6,
                                            start_offset: "0.100s", end_offset: "0.400s" } } }
```

`output_text` 和上次的 `output_audio` 一樣，是 Python SDK 從 `steps` 裡組出來的便利欄位。這次在寫程式之前就知道了，所以直接從 `steps[].content[]` 讀。

## 詞的位置是用 UTF-8 位元組算的

「今日」這個詞的 `start_index` 是 0、`end_index` 是 6，不是 0 到 2。**位置是用 UTF-8 的位元組計算的**，一個中日文字元佔 3 個位元組。如果照 JavaScript 字串的索引去切，日文會全部錯位。

這兩件事都只有看過真實回應才會知道。這次花了兩次 API 呼叫，換來的是上線後不用再猜。

---

# 怎麼判斷「念對了」

## 用 verbatim，不用 smart

官方主打的 `smart` 模式會處理說話時的自我修正、拿掉語助詞。對會議記錄來說這是優點，對跟讀卻是缺點：**你念錯了又改口，smart 可能只留下改好的版本；你漏念了一個助詞，它可能幫你補得很通順。**跟讀要的是「你實際念了什麼」，所以用 `verbatim`。

## 不用自訂詞彙

把這句的原文設成自訂詞彙，看起來能提高辨識率，但它會讓模型**偏向聽成正確答案**，反而抓不到念錯的地方。而且文件寫明，自訂詞彙和逐字時間戳不能同時使用。

## 讀音和寫法都比

轉錄結果可能寫成漢字「今日」，也可能寫成假名「きょう」，兩種都是念對。只比對文字的話，會把念對的判成錯。

所以日文同時用兩種方式比對，**任一方式對得上就算念對**：

- **讀音**：把轉錄結果用 pykakasi 轉成平假名，和這句已經標註好的讀音比對。
- **寫法**：直接比對文字，防止 pykakasi 把某個漢字念錯。

比對本身用 Python 內建的 `difflib.SequenceMatcher` 做字元對齊，再把結果對回每個詞。英文和韓文則以詞為單位比對。

## 分清楚「念錯」和「沒念到」

第一版只看「這個詞的字有沒有對上」，結果「今日は雨です」的「晴れ」被標成**沒念到**，但其實是**念成了別的詞**。

改成記錄每個字的對齊結果（相符、被替換、被刪除）之後才分得開：

| 情況 | 轉錄結果 | 判定 |
|---|---|---|
| 念對 | 今日は晴れです。 | 全部念對，100 分 |
| 換掉一個詞 | 今日は雨です。 | 「晴れ」念錯，75 分 |
| 跳過中間的詞 | 今日はです | 「晴れ」沒念到，75 分 |
| 念到一半 | 今日は | 「晴れ」「です」沒念到，50 分 |
| 轉錄成假名 | きょうははれです | 全部念對 |
| 片假名加全形符號 | キョウハ　ハレデス！ | 全部念對 |
| 前後多了語助詞 | えっと今日は晴れですね | 全部念對 |

這些都是不呼叫 API 的單元測試，每次修改比對邏輯都能馬上跑一遍。

---

# 先講清楚限制

語音轉文字檢查的是「**別人聽不聽得出你念的是哪個詞**」，不是精細的發音評分：

- 稍微不標準、但還聽得出是正確的詞，**會被判成念對**。模型會往最可能的詞去猜，這是語音轉文字的本質。
- 長音、促音「っ」、音調高低這些細節，只有錯到變成另一個詞時才會被抓到。

所以卡片上的說明直接寫著：「只檢查聽不聽得出是哪個詞，音調與長短音不在評分範圍」。之後如果想要更細的講評，可以另外把錄音和老師的示範音一起交給 `gemini-3.8-flash`，請它用文字說明，但那是另一個功能。

---

# 踩坑一：按鈕在麥克風還沒準備好時，就說「錄音中」

第一版的流程是：點「跟讀」→ 按鈕立刻變成「■ 停止並評分」→ 開始要求麥克風。

問題在於**第一次使用時，瀏覽器會跳出麥克風權限的詢問**。這時按鈕已經顯示「停止並評分」，但麥克風根本還沒開始錄。如果這時再按一次，程式會以為還沒開始，於是**又要求一次麥克風**，而不是停止。

**原因與解法**：多一個「準備麥克風中」的狀態。按下之後按鈕先顯示「準備麥克風…」並暫時不能按，**等 `MediaRecorder` 真的開始錄了，才切換成「停止並評分」**。

順帶一提，issue 裡原本寫的是「按住錄音、放開送出」，實作時改成「點一下開始、再點一下停止」，原因也是這個權限詢問：按住的話，手指一定會在按權限按鈕時放開，第一次永遠錄不到。再加上 iPhone 長按按鈕容易觸發文字選取，念一整句歌詞也要好幾秒，點兩下比按住好用。

---

# 踩坑二：Mac 上的無頭 Chrome 拿不到麥克風

端對端測試我一直用 puppeteer 開無頭 Chrome。Chrome 有一組測試參數可以「拿一個音檔假裝成麥克風」：

```
--use-fake-ui-for-media-stream
--use-fake-device-for-media-stream
--use-file-for-fake-audio-capture=ok.wav
```

結果 `getUserMedia` 一直卡住不回應。換成 48kHz 的 wav、不指定音檔、預先授予麥克風權限，全部一樣。連最基本的假裝置都拿不到，看起來是 macOS 的麥克風權限機制擋住了無頭 Chrome。

**原因與解法**：與其卡在測試環境，不如換一個切入點。在頁面載入前替換掉 `getUserMedia`，把測試音檔解碼後用 Web Audio 播放，產生一條真的音訊串流：

```js
navigator.mediaDevices.getUserMedia = async () => {
  const ctx = new AudioContext({ sampleRate: 48000 });
  const buffer = await ctx.decodeAudioData(testWavBytes.buffer);
  const src = ctx.createBufferSource();
  src.buffer = buffer;
  const dest = ctx.createMediaStreamDestination();
  src.connect(dest);
  src.start();
  return dest.stream;
};
```

除了「系統麥克風」這一段，錄音（`MediaRecorder` 錄成 webm/opus）、上傳、轉錄、比對、顯示結果整條流程都是真的。測試結果是 100 分、四個詞都是綠色，只送出一次評分請求，伺服器的暫存資料夾也沒有殘留檔案。

踩坑一的 bug，也是在這個測試的第一個版本裡抓到的：按鈕顯示「停止並評分」，但 `MediaRecorder` 從來沒有被建立過。

---

# 隱私與額度

跟讀會碰到兩樣敏感的東西：**我的錄音**，以及 **API 額度**。

| 設計 | 做法 |
|---|---|
| 錄音不保存 | 只在評分時寫到伺服器的暫存資料夾，評完立刻連資料夾一起刪掉（寫在 `finally` 裡），不進 bucket，也不寫 log |
| 不信任瀏覽器傳來的歌詞 | 伺服器依歌曲 ID 和句子編號，自己去讀這句的原文和讀音 |
| 不重複計費 | 同一時間只處理一次評分；SDK 的自動重試關掉 |
| 額度用完就停 | 收到 429 之後，在 `Retry-After` 指定的時間內直接拒絕，不再呼叫 Gemini |
| 沒登入的人碰不到 | 跟讀的 API 一樣在 IAP 後面，沒登入的請求在 IAP 那一層就被擋下，連程式都進不來，也就不會花到額度 |

錄音送給 Gemini 轉錄是這個功能本身必要的，但在自己的服務這一側，錄音存在的時間只有那一次呼叫的幾秒鐘。

---

# 順帶：讓 PWA 在 IAP 後面裝得起來

roadmap 的第一項是手機版面：MV 固定在上方、卡片可以左右滑動換句、拇指按得到的底部控制列，外加可以加入主畫面的 PWA。版面本身沒什麼特別，但 PWA 在 IAP 後面有一個坑值得記一下。

瀏覽器抓 PWA 的設定檔（manifest）時，**預設不帶 cookie**。在 IAP 後面，沒有 cookie 的請求會被導向 Google 登入頁，manifest 就讀不到，PWA 也就裝不起來。

HTML 的解法是在 `<link rel="manifest">` 加上 `crossorigin="use-credentials"`。但翻 Next.js 16 的原始碼才發現，它內建的 manifest 連結**只有在 Vercel 的預覽環境才會加這個屬性**：

```js
crossOrigin: !manifestOrigin && process.env.VERCEL_ENV === 'preview' ? 'use-credentials' : undefined
```

**原因與解法**：不用 Next 內建的 `app/manifest.ts`，改用一般的 route 提供 manifest，自己在 `<head>` 放帶 `use-credentials` 的連結。manifest 裡的圖示也直接內嵌成 data URL，免得瀏覽器再發一次不帶 cookie 的請求。

---

# 成果與效益

| | 數字 |
|---|---|
| 這次的 commit | 2（手機版面與 PWA、跟讀評分） |
| repo 總 commit | 18 |
| 關閉的 issue | 2 / 3（roadmap 剩單字卡） |
| 一次跟讀評分 | 約 9–10 秒（上傳約 2 秒、轉錄約 4 秒，其餘是啟動 Python） |
| 一次評分的用量 | 約 40 個 token，不占 TTS 額度 |
| 驗證 API 花掉的呼叫 | 5–6 次 transcribe |

現在在手機上學一首歌的流程是：看卡片 → 聽老師念 → 按跟讀自己念 → 看哪個詞沒念對 → 聽自己的錄音和老師對照 → 滑到下一句。

## 幾個我會帶走的東西

**「先看真實回應」要寫成規則，不能靠記得。**這次在 issue 裡明確寫了這條，開工前就先打了兩次 API。結果 `output_text` 和 UTF-8 位元組這兩件事，都在寫第一行程式之前就知道了。

**官方主打的模式，不一定適合你的用途。** `smart` 模式對大部分場景都是加分，但跟讀要的恰好是它會修掉的東西。選模式之前，先想清楚你要的是「整理好的結果」還是「實際發生的事」。

**能幫你提高準確度的功能，可能正好在幫倒忙。**自訂詞彙會讓模型偏向正確答案，而評分需要的是能抓到錯誤。

**UI 狀態要反映真實狀態，而不是你以為的狀態。**按鈕說「錄音中」的時候，麥克風其實還沒開，這種落差在第一次授權的那一刻最明顯，而那剛好是使用者最容易亂按的時候。

**測試環境卡住時，換一個切入點。**與其跟 macOS 的麥克風權限搏鬥，不如把「系統麥克風」這一小段替換掉，讓其他部分全部照真的跑。

**先講清楚能做到什麼。**語音轉文字檢查的是「聽不聽得懂」，不是發音評分。把這個限制直接寫在畫面上，比讓使用者以為拿了 100 分就代表發音完美誠實得多。

程式碼在 [kkdai/song-lingo](https://github.com/kkdai/song-lingo)，跟讀評分的實作在 `shadow.py` 與 `web/app/api/songs/[id]/lines/[index]/shadow/`。官方資料：[Gemini 3.5 Transcribe 發布文章](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-5-transcribe/)、[Transcribe 文件](https://ai.google.dev/gemini-api/docs/transcribe)。
