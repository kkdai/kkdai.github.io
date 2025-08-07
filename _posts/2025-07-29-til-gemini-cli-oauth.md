---
layout: post
title: "[Python][Gemini CLI] 使用 LangChain 中的 Vertex AI 來處理 LINE Bot 中的圖片內容"
description: ""
category: 
- TIL
tags: ["GCP", "python", "LangChain"]

---


# 前情提要




## 關於 Vertex AI 上面使用 Gemini 來做圖形偵測的架構圖









絕對可以！您的想法完全正確，這是解決這個問題的標準且唯一的途徑。

  既然不能用一個「超級管理員」帳號（服務帳號）來處理所有事情，我們就必須讓每一個使用者授權您的應用程式來存取他們自己的 Google
  Drive。這就是 OAuth 2.0 的用途。

  這個改動會比之前複雜很多，因為它引入了使用者互動的認證流程。我會為您詳細拆解。

  宏觀概念：使用者授權流程

   1. 使用者發起連線：使用者在 Telegram 中對您的機器人發送一個指令，例如 /connect_drive。
   2. 產生授權連結：您的機器人會產生一個獨一無二的 Google 授權連結，並將它傳回給使用者。
   3. 使用者授權：使用者點擊連結，會被帶到 Google 的登入和授權頁面。頁面會顯示「TG Helper 想要存取您的 Google
      Drive」，使用者點擊「允許」。
   4. 重新導向並取得授權碼：Google 會將使用者重新導向到您預先設定好的一個網址（Callback URL），並附上一組一次性的授權碼 (Authorization
      Code)。
   5. 交換權杖：您的後端服務（Cloud Run）收到這個授權碼後，會用它跟 Google 交換一組存取權杖 (Access Token) 和一個非常重要的更新權杖
      (Refresh Token)。
   6. 儲存權杖：您的應用程式需要一個資料庫（例如 Firestore），將使用者的 Telegram ID 和他們的更新權杖安全地儲存起來。
   7. 上傳檔案：當這位使用者之後傳送檔案時，您的機器人會：
       * 從資料庫中讀取這位使用者的更新權杖。
       * 使用更新權杖取得一組新的、有時效性的存取權杖。
       * 使用這組代表該使用者的存取權杖，將檔案上傳到他自己的 Google Drive。

  新的架構

  這個流程需要一些新的元件：

   * 一個 Web 端點：您的 Cloud Run 應用程式除了接收 Telegram 的 Webhook，還需要一個新的 HTTP 路徑 (例如 /oauth/callback) 來接收 Google
     的重新導向。
   * 一個資料庫：用來儲存使用者 ID 和他們的 Refresh Token。最簡單的選擇是 Firestore，因為它與 Cloud Run 整合得很好，而且有免費額度。

---

  實作步驟

  步驟 1：Google Cloud Console 設定

   1. 設定 OAuth 同意畫面
       * 前往 OAuth 同意畫面 (https://console.cloud.google.com/apis/credentials/consent)。
       * 使用者類型：選擇「外部」。
       * 填寫應用程式名稱（例如 TG Helper Bot）、使用者支援電子郵件等資訊。
       * 範圍：點擊「新增或移除範圍」，找到並加入
         .../auth/drive.file。這個範圍代表「僅允許上傳由本應用程式建立的檔案」，是權限最小、最安全的選擇。
       * 儲存並繼續。在「測試使用者」步驟中，將您自己的 Gmail 加入，這樣在應用程式發布前您才能進行測試。

   2. 建立 OAuth 2.0 用戶端 ID
       * 前往 憑證頁面 (https://console.cloud.google.com/apis/credentials)。
       * 點擊「建立憑證」 > 「OAuth 用戶端 ID」。
       * 應用程式類型：選擇「網頁應用程式」。
       * 已授權的重新導向 URI：這是最關鍵的一步。點擊「新增 URI」，然後輸入您的 Cloud Run 服務網址，並在後面加上一個回呼路徑，例如：
          https://tg-helper-xxxx-an.a.run.app/oauth/callback
       * 點擊「建立」。
       * 建立後，您會得到一組用戶端 ID (Client ID) 和用戶端密鑰 (Client Secret)。請將這兩者複製下來，它們非常重要。

  步驟 2：啟用 Firestore 並安全地儲存密鑰

   1. 啟用 Firestore API

   1     gcloud services enable firestore.googleapis.com
   2. 建立 Firestore 資料庫
       * 前往 Firestore 主控台 (https://console.cloud.google.com/firestore)。
       * 選擇「原生模式」(Native mode)。
       * 選擇一個離您最近的位置。
       * 建立資料庫。

   3. 安全地儲存 Client Secret
      我們應該再次使用 Secret Manager 來儲存您在步驟 1-2 取得的 Client Secret。

   1     # 將您的 Client Secret 貼在引號中
   2     echo -n "YOUR_COPIED_CLIENT_SECRET" | gcloud secrets create tg-bot-client-secret --data-file=-

  步驟 3：修改 Go 程式碼 (概念)

  這將是一個較大的重構。我先不提供完整程式碼，而是描述邏輯上的變更：

   1. 新增 `/connect_drive` 指令處理：
       * 當收到此指令，程式會使用 Google 的 Go Auth 函式庫產生一個授權 URL。
       * 這個 URL 會包含您的 Client ID、redirect_uri、scope，以及一個 state 參數。state 參數通常會包含該使用者的 Telegram
         ID，以便在回呼時能識別是誰。
       * 機器人將此 URL 回傳給使用者。

   2. 新增 `/oauth/callback` HTTP 處理器：
       * 這個函式會處理來自 Google 的重新導向。
       * 它會從 URL 參數中取得 code 和 state。
       * 驗證 state 以確保請求的合法性，並從中解析出 Telegram User ID。
       * 使用 code 和您的 Client Secret 向 Google 交換權杖。
       * 將 (Telegram User ID, Refresh Token) 這組對應關係存入 Firestore。
       * 向使用者顯示一個「授權成功！」的簡單網頁。

   3. 重構 `handleFile` 函式：
       * 當收到檔案時，它會先用檔案傳送者的 Telegram User ID 去 Firestore 查詢。
       * 如果找不到對應的 Refresh Token，就回覆訊息請使用者先執行 /connect_drive。
       * 如果找到了，就用 Refresh Token 取得一個新的 Access Token。
       * 用這個 Access Token 建立一個 Drive 服務，然後上傳檔案。
