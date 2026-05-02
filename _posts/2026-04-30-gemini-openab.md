---
layout: post
title: "[GCP 實戰] OpenAB 部署筆記：在 GCE 上打造支援 Telegram 的 Gemini ACP 橋接器"
description: "紀錄如何將 OpenAB 部署於 Google Compute Engine，並透過自定義 Dockerfile 整合 Gemini-cli，解決 Telegram Webhook 的 HTTPS 需求與持續性 Session 的挑戰。"
category:
- AI
- GCP
tags: ["Google Cloud", "Compute Engine", "OpenAB", "Gemini", "Telegram", "Docker", "Cloudflare"]
---



![image-20260502171732526](../images/image-20260502171732526.png)

# 前情提要

最近為了讓 AI 編碼助理（如 Claude Code 或 Gemini CLI）能直接在聊天平台上使用，我開始研究 **[OpenAB](https://openabdev.github.io/openab/) **。這是一個強大的橋接器，能將 Slack、Discord 或 Telegram 對接到符合 **ACP (Agent Client Protocol)** 標準的 CLI 工具。

這篇文章紀錄了我將 [OpenAB](https://openabdev.github.io/openab/) 部署在 Google Cloud 上的完整實戰過程，特別是如何繞過認證限制、處理 Telegram 的 HTTPS 需求，以及解決容器化部署中的路徑與權限問題。

- **OpenAB 參考文件**： [https://openabdev.github.io/openab/](https://openabdev.github.io/openab/)
- **OpenAB Repo**: [https://github.com/openabdev/openab](https://github.com/openabdev/openab)

---

## 部署決策：為什麼選 GCE 而不是 Cloud Run？

雖然 Cloud Run 是我的首選，但在處理 OpenAB 時，**Google Compute Engine (GCE)** 才是最佳解決方案。原因有二：

1. **Stateful Session (狀態化對話)**：OpenAB 會為每個對話 thread 啟動一個子進程（如 Gemini CLI）。這些進程必須長期駐留以維持對話上下文。Cloud Run 的自動縮減機制會殺死這些進程，導致對話中斷。
2. **認證持久化**：AI CLI 的 Token 需要儲存在本地磁碟。GCE 配合 Persistent Disk 能保證重啟後登入狀態不消失。

---

## 實戰步驟：Step-by-Step 部署過程

### 第一步：撰寫自動化啟動腳本 (Startup Script)

為了讓部署標準化，我們撰寫了一個 `setup-openab.sh`。其核心任務是安裝 Docker、建立持久化目錄，並動態生成 `config.toml`。

最關鍵的部分是 **自定義 Docker Image**。由於 OpenAB 官方鏡像不一定包含所有 AI 工具，我們透過 Dockerfile 現場安裝 Node.js 與 `@google/gemini-cli`：

```dockerfile
FROM ghcr.io/openabdev/openab:latest
USER root
RUN apt-get update && apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g @google/gemini-cli
USER 1000
```

### 第二步：使用 gcloud 建立 GCE 實例

我們選擇 `e2-medium` 規格，並透過 Metadata 傳遞敏感資訊（如 Bot Token），避免寫死在腳本中。

```bash
gcloud compute instances create openab-server \
    --project=your-project-id \
    --zone=asia-east1-b \
    --machine-type=e2-medium \
    --image-family=debian-11 \
    --image-project=debian-cloud \
    --metadata-from-file startup-script=setup-openab.sh \
    --metadata=tg_bot_token=YOUR_BOT_TOKEN
```

### 第三步：配置 Gemini API Key

不同於 Kiro 需要互動式登入，`gemini-cli` 可以直接讀取環境變數。我們將 API Key 注入到 OpenAB 的 `config.toml` 中，讓它在背景自動運作：

```toml
[agent]
command = "gemini"
args = ["--acp"]
env = { GEMINI_API_KEY = "AIzaSy..." }
```

### 第四步：使用 Cloudflare Tunnel 解決 HTTPS 需求

Telegram Webhook 強制要求 **HTTPS**。與其設定複雜的 Nginx + SSL，我選擇使用 **Cloudflare Quick Tunnel**：

1. 在 VM 執行：`cloudflared tunnel --url http://localhost:8080`。
2. 取得隨機生成的 HTTPS 網址。
3. 註冊 Webhook：`curl "https://api.telegram.org/bot<TOKEN>/setWebhook?url=<CF_URL>/webhook/telegram&secret_token=<SECRET>"`。

---

## 遷移過程中的血淚踩坑：技術總結

在部署過程中，我們來回除錯了幾次，以下是總結出的三大「坑」：

### 踩坑一：鏡像來源的混淆

一開始我嘗試從 Docker Hub Pull `openabdev/openab` 但一直失敗。最後才發現該專案目前的穩定鏡像是放在 **GitHub Container Registry (GHCR)**。
*   **解法**：必須使用 `ghcr.io/openabdev/openab:latest`。

### 踩坑二：硬編碼的配置路徑

OpenAB 的 Dockerfile 內部預期設定檔路徑在 `/etc/openab/config.toml`。我最初掛載到 `/app/config.toml` 導致容器啟動後立即閃退報錯。
*   **解法**：修正 Docker Volume 掛載路徑為 `/etc/openab/config.toml`。

### 踩坑三：Security Secret Token 驗證失敗

即便 URL 正確，Telegram 訊息仍被 Gateway 拒絕。日誌顯示 `invalid or missing secret_token`。
*   **原因**：`openab-gateway` 為了防止非法請求，會生成一個內部校驗碼。
*   **解法**：必須從 Gateway 容器中提取該 Token，並在 `setWebhook` 時作為 `secret_token` 參數傳遞。

---

## 總結：完美的 AI 橋接方案

透過這套架構，我成功在 GCP 上建立了一個完全自託管、安全且高效率的 AI 助理。它不依賴昂貴的訂閱，而是直接利用 Gemini 的 API 能力，並透過 Telegram 作為互動介面。

如果你也想在雲端架設一個專屬的 ACP 橋接器，這套 GCE + Docker + Cloudflare Tunnel 的組合方案將是最平衡穩定選擇。
