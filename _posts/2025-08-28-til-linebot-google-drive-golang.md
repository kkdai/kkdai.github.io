---
layout: post
title: "[Gemini][Golang] 打造一個用 Golang 開發的 LINE 檔案備份機器人到 Google Drive"
description: ""
category: 
- TIL
tags: ["Gemini_CLI", "Golang", "LINEBot"]

---

![image-20250829153659791](../images/image-20250829153659791.png)



在數位時代，管理 LINE 聊天中的照片、影片、音訊和檔案可能有點麻煩，尤其是想讓這些資料井然有序。這時候，[kkdai/linebot-file](https://github.com/kkdai/linebot-file) 這個開源專案就能派上用場！這款功能強大的 LINE 機器人使用 **Golang** 開發，能自動將聊天室中的多媒體檔案備份到你的 Google Drive，還能聰明整理資料夾並提供簡單的查詢功能。這篇技術部落格將帶你認識這個機器人的核心功能，並一步步教你如何在 Google Cloud Platform (GCP) 上部署它。

## ✨ 主要功能介紹

這個用 Golang 打造的 LINE 機器人提供了以下超實用的功能：

- **多媒體檔案備份**：支援將圖片、影片、音訊和一般檔案從 LINE 聊天備份到 Google Drive。
- **聰明資料夾整理**：自動在 Google Drive 建立一個 `LINE Bot Uploads` 資料夾，並以年月（`YYYY-MM`）為單位建立子資料夾，讓你的雲端硬碟乾淨整齊。
- **安全帳號連結**：採用 Google OAuth 2.0 授權，確保連線安全又可靠。
- **快速查詢檔案**：輸入 `/recent_files` 指令，就能快速查看最近上傳的 5 個檔案。
- **靈活連線控制**：隨時透過 `/disconnect_drive` 指令斷開 Google Drive 連線並撤銷授權。

## 上傳檔案到 Google Drive 的兩種方式



## 🚀 在 Google Cloud Platform 上部署

這個專案使用 Golang 開發，並已容器化（包含 Dockerfile），非常適合部署在 Google Cloud Run 上。Cloud Run 提供全代管的無伺服器環境，自動擴展超方便！以下是部署的完整步驟。

### 前置準備

開始之前，請確認你已經準備好：

- 一個 Google Cloud 帳號。
- 安裝並設定好 Google Cloud SDK（gcloud CLI）。
- 一個 LINE Bot 頻道，並取得 Channel Secret 和 Channel Access Token。

### 部署步驟

#### 1. 啟用必要 API

為了讓機器人正常運作，需要在 Google Cloud 專案中啟用以下服務：

- Cloud Run API
- Cloud Build API（用來自動建置容器映像檔）
- Firestore API（用來儲存使用者授權資料）

在終端機執行以下指令快速啟用：

```bash
gcloud services enable run.googleapis.com cloudbuild.googleapis.com firestore.googleapis.com
```

#### 2. 建立 Firestore 資料庫

- 前往 Google Cloud Console 的 Firestore 頁面。
- 選擇「原生模式（Native mode）」。
- 挑選離你的使用者最近的地區，然後建立資料庫。

#### 3. 取得 Google OAuth 憑證

這是讓機器人能存取 Google Drive 的關鍵步驟：

- 前往 Google Cloud Console > APIs & Services > Credentials。
- 點擊 **+ CREATE CREDENTIALS**，選擇 **OAuth client ID**。
- 在 Application type 中選 **Web application**，並命名（例如「LINE Bot File Uploader」）。
- 這一步先不要填寫 **Authorized redirect URIs**，等 Cloud Run 部署完成後再回來設定。
- 建立後，你會得到一組 **Client ID** 和 **Client Secret**，請妥善保存，後面會用到。



#### 4. 部署到 Cloud Run

將專案程式碼 clone 到你的本地環境，然後在專案根目錄執行以下指令：

```bash
gcloud run deploy linebot-file-service \
  --source . \
  --platform managed \
  --region asia-east1 \
  --allow-unauthenticated \
  --set-env-vars="ChannelSecret=YOUR_CHANNEL_SECRET" \
  --set-env-vars="ChannelAccessToken=YOUR_CHANNEL_ACCESS_TOKEN" \
  --set-env-vars="GOOGLE_CLIENT_ID=YOUR_GOOGLE_CLIENT_ID" \
  --set-env-vars="GOOGLE_CLIENT_SECRET=YOUR_GOOGLE_CLIENT_SECRET" \
  --set-env-vars="GOOGLE_REDIRECT_URL=YOUR_CLOUD_RUN_URL/oauth/callback"
```

**參數說明**：

- `linebot-file-service`：你的 Cloud Run 服務名稱，可自訂。
- `--region`：建議選離你最近的地區，例如 `asia-east1`（台灣）。
- `--allow-unauthenticated`：允許來自 LINE Platform 的公開請求。
- `YOUR_...`：替換成你的金鑰和憑證。
- `GOOGLE_REDIRECT_URL`：先填一個臨時網址（例如 `https://temp.com`）。

#### 5. 設定 Webhook 和 Redirect URI

- 部署完成後，Cloud Run 會提供一個服務 URL（例如 `https://linebot-file-service-xxxxxxxx-an.a.run.app`）。
- **更新 LINE Webhook**：前往 LINE Developers Console，將你的 Bot 頻道 Webhook URL 設為 Cloud Run 服務 URL。
- **更新 Google OAuth Redirect URI**：回到步驟 3 的憑證頁面，編輯 Web application 憑證，在 Authorized redirect URIs 加入 `YOUR_CLOUD_RUN_URL/oauth/callback`（例如 `https://linebot-file-service-xxxxxxxx-an.a.run.app/oauth/callback`）。
- **重新部署 Cloud Run**：再次執行步驟 5 的 `gcloud run deploy`，這次將 `GOOGLE_REDIRECT_URL` 更新為正確的 Cloud Run 回呼網址。

（在這邊你可以貼上你的 Golang 程式碼中處理 Webhook 的部分，例如如何解析 LINE Webhook 事件或處理檔案上傳到 Google Drive 的邏輯，這樣可以讓讀者看到實際的 Golang 實作細節。）

## 未來展望

這個用 Golang 開發的 LINE 檔案備份機器人開啟了許多有趣的應用場景：

- **自動化檔案管理**：讓你的 LINE 聊天檔案自動整理到 Google Drive，省時又省力。
- **團隊協作**：在群組聊天中快速備份和分享檔案，提升工作效率。
- **個人化功能**：根據需求擴充功能，例如自訂資料夾結構或檔案命名規則。
- **數據分析**：分析上傳檔案的類型和頻率，優化機器人功能或提供使用統計。

（如果你有其他 Golang 相關的程式碼片段，例如處理 Google Drive API 或 Firestore 的部分，可以在這裡貼上，進一步展示 Golang 在這個專案中的應用。）

這些應用不僅能提升個人和團隊的檔案管理體驗，還能為開發者帶來更多創意應用的靈感。快動手部署你的 LINE 檔案備份機器人，體驗 Golang 和雲端整合的便利吧！
