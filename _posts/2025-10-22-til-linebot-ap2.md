---
layout: post
title: "[Gemini][Python] LINE Bot 跟 Google 自動化支付整合試試看 Agent Payments Protocol (AP2)"
description: ""
category: 
- TIL
tags: ["AP2", "Python", "LINEBot"]

---

![Agent Payments Protocol Graphic](https://github.com/google-agentic-commerce/AP2/raw/main/docs/assets/ap2_graphic.png)

現在 Agent 的相關框架相當的多，但是其實 Google 在日前也宣布了一個蠻有趣的傳輸協定 Agent Payments Protocol (AP2) ，用來做進銷存管理跟支付的一套 agent framework 。





![LINE 2025-10-22 08.31.18](../images/LINE 2025-10-22 08.31.18.png)







## 範例程式碼

#### [https://github.com/kkdai/linebot-ap2](https://github.com/kkdai/linebot-ap2)

歡迎給 Star 與分享，如果覺得實用也歡迎參與貢獻添加一些新功能。



## ✨ 主要功能介紹

這個用 Golang 打造的 LINE 機器人提供了以下超實用的功能：

- **多媒體檔案備份**：支援將圖片、影片、音訊和一般檔案從 LINE 聊天備份到 Google Drive。
- **聰明資料夾整理**：自動在 Google Drive 建立一個 `LINE Bot Uploads` 資料夾，並以年月（`YYYY-MM`）為單位建立子資料夾，讓你的雲端硬碟乾淨整齊。
- **安全帳號連結**：採用 Google OAuth 2.0 授權，確保連線安全又可靠。
- **快速查詢檔案**：輸入 `/recent_files` 指令，就能快速查看最近上傳的 5 個檔案。
- **靈活連線控制**：隨時透過 `/disconnect_drive` 指令斷開 Google Drive 連線並撤銷授權。

# LINE Bot 檔案備份到 Google Drive 的兩種上傳方式比較

這個用 Golang 寫成的 LINE 檔案備份機器人，讓使用者能輕鬆將聊天室中的檔案備份到 Google Drive。專案支援兩種上傳 Google Drive 的方式：**Google Cloud Service Account** 和 **Google OAuth 2.0**。以下是一張比較表格，詳細說明這兩種方式的運作方式、優點與缺點，幫助開發者選擇最適合的實作方式。

## 比較表格

| **項目**           | **Google Cloud Service Account**                             | **Google OAuth 2.0**                                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **運作方式**       | 使用 Google Cloud 專案中的服務帳戶憑證（JSON 檔案），代表應用程式直接存取 Google Drive API，無需使用者手動授權。 | 使用者透過 OAuth 2.0 流程授權，允許機器人以使用者的身份存取其 Google Drive 帳戶。 |
| **設定複雜度**     | 中等：需在 Google Cloud Console 建立服務帳戶、生成憑證，並分享特定 Google Drive 資料夾給服務帳戶。 | 較高：需設定 OAuth 同意畫面、建立 Web application 憑證，並處理回呼網址和權杖管理。 |
| **使用者體驗**     | 無需使用者介入，檔案直接上傳到預設的 Google Drive 資料夾（通常由服務帳戶擁有或共享）。 | 使用者需手動點擊授權連結完成 OAuth 流程，授權後檔案上傳到使用者自己的 Google Drive。 |
| **權限控制**       | 服務帳戶擁有固定的存取權限，需手動分享資料夾給服務帳戶，權限管理較集中。 | 使用者可控制授權範圍（如僅允許存取特定資料夾），並可隨時撤銷權限（`/disconnect_drive`）。 |
| **主要優點**       | - 自動化程度高，無需使用者手動授權。 - 適合集中管理檔案（例如企業場景）。 - 部署後幾乎無需使用者互動。 | - 使用者擁有檔案完全控制權，符合個人化需求。 - 支援動態權限管理，安全性更高。 - 適合多使用者場景，每人備份到自己的 Google Drive。 |
| **主要缺點**       | - 檔案儲存在服務帳戶的 Google Drive 或共享資料夾，個人使用者可能無法直接管理。 - 需額外設定資料夾分享，增加初始配置複雜度。 - 不適合多使用者場景（除非為每個使用者設定獨立資料夾）。 | - 使用者需完成 OAuth 授權流程，影響初始體驗。 - 需管理權杖更新（refresh token），程式碼較複雜。 - 部署時需處理回呼網址，增加設定步驟。 |
| **適合場景**       | 企業或集中式應用，管理者希望統一管理所有備份檔案。           | 個人化應用，使用者希望檔案儲存在自己的 Google Drive 並保留完全控制權。 |
| **程式碼實作範例** | （在這邊你可以貼上使用 Google Cloud Service Account 的 Golang 程式碼片段，例如初始化服務帳戶並上傳檔案的邏輯。） | （在這邊你可以貼上使用 Google OAuth 2.0 的 Golang 程式碼片段，例如處理 OAuth 流程和上傳檔案的邏輯。） |

## 兩種方式的詳細說明

### Google Cloud Service Account

- **運作原理**：服務帳戶是 Google Cloud 提供的一種非人類帳戶，透過 JSON 憑證檔案進行身份驗證。你的 LINE 機器人使用服務帳戶的憑證直接呼叫 Google Drive API，將檔案上傳到指定的資料夾。這個資料夾可以是服務帳戶自己的 Google Drive，或是管理者分享給服務帳戶的資料夾。
- **優點**：
  - 無需使用者介入，適合全自動化流程。
  - 適合企業場景，例如所有員工的 LINE 聊天檔案備份到公司統一管理的 Google Drive。
  - 程式碼相對簡單，只需初始化服務帳戶並設定 API 呼叫。
- **缺點**：
  - 檔案儲存在服務帳戶的 Google Drive 或共享資料夾，使用者無法直接管理（除非透過共享權限）。
  - 每個使用者若需獨立資料夾，需額外程式邏輯來動態管理資料夾分享。
  - 初始設定需手動分享 Google Drive 資料夾給服務帳戶的電子郵件地址。

（你可以在這裡貼上服務帳戶的 Golang 程式碼，例如使用 `google.golang.org/api/drive/v3` 套件初始化服務帳戶並上傳檔案的片段。）

### Google OAuth 2.0

- **運作原理**：使用者透過 LINE 機器人點擊 `/connect_drive` 指令，觸發 OAuth 2.0 授權流程，允許機器人存取其 Google Drive。機器人會取得存取權杖（access token）和更新權杖（refresh token），用以代表使用者上傳檔案到其個人 Google Drive。
- **優點**：
  - 使用者擁有檔案的完全控制權，檔案直接儲存在自己的 Google Drive。
  - 支援動態權限管理，使用者可隨時撤銷授權（透過 `/disconnect_drive`）。
  - 適合個人化應用，特別是多使用者場景，每人備份到自己的 Google Drive。
- **缺點**：
  - 使用者需手動完成 OAuth 授權流程，可能影響初始體驗。
  - 程式碼需處理權杖管理（例如更新過期的 access token），增加開發複雜度。
  - 部署時需設定回呼網址（redirect URI），並確保與 Cloud Run 的 URL 一致。

（你可以在這裡貼上 OAuth 的 Golang 程式碼，例如使用 `golang.org/x/oauth2` 套件處理授權流程和檔案上傳的片段。）



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

- 前往 Google Cloud Console > APIs & Services > Credentials。 ([Google Auth Platform](https://console.cloud.google.com/auth))
- 點擊 **+ CREATE CREDENTIALS**，選擇 **OAuth client ID**。

![image-20250829161538979](../images/image-20250829161538979.png)

- 在 Application type 中選 **Web application**，並命名（例如「LINE Bot File Uploader」）。
- 這一步先不要填寫 **Authorized redirect URIs**，等 Cloud Run 部署完成後再回來設定。

![image-20250829161558554](../images/image-20250829161558554.png)

- 建立後，你會得到一組 **Client ID** 和 **Client Secret**，請妥善保存，後面會用到。





## 成果展示

![image-20250829162044191](../images/image-20250829162044191.png)

- 如果沒有認證過，會出現請認證的說明。
- 點選網址認證 Google Drive 的上傳權限即可。
- 如果不想使用，也可以使用  `/disconnect_drive` 來撤銷相關授權。



![image-20250829161720900](../images/image-20250829161720900.png)

- 上傳檔案也很簡單，支援兩種格式。
  - 直接上傳圖片，會將圖片上傳到 google drvie 保存。
  - 透過 iOS / AOS 將 PDF 或是任何檔案格式，轉貼到對話視窗。也可以精準上傳到 Google Drive。檔案也可以超過 50MB
- 要看檔案，就點選查詢最近檔案，就會開啟 Google Drive 網頁去查看檔案列表。

## 未來展望

這個用 Golang 開發的 LINE 檔案備份機器人開啟了許多有趣的應用場景：

- **自動化檔案管理**：讓你的 LINE 聊天檔案自動整理到 Google Drive，省時又省力。
- **團隊協作**：在群組聊天中快速備份和分享檔案，提升工作效率。
- **個人化功能**：根據需求擴充功能，例如自訂資料夾結構或檔案命名規則。
- **數據分析**：分析上傳檔案的類型和頻率，優化機器人功能或提供使用統計。

（如果你有其他 Golang 相關的程式碼片段，例如處理 Google Drive API 或 Firestore 的部分，可以在這裡貼上，進一步展示 Golang 在這個專案中的應用。）

這些應用不僅能提升個人和團隊的檔案管理體驗，還能為開發者帶來更多創意應用的靈感。快動手部署你的 LINE 檔案備份機器人，體驗 Golang 和雲端整合的便利吧！
