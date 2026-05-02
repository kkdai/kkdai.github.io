---
layout: post
title: "[GCP 實戰] 打造持久化 AI 助手：在 GCE 部署 Hermes Agent 並接軌 Telegram"
description: "紀錄在 Google Compute Engine (GCE) 上部署 NousResearch Hermes Agent 的完整過程，包含 Gemini 2.5 Flash 模型配置、Telegram Gateway 自動化設定，以及解決模型 404 與背景執行持久化的實戰經驗。"
category:
- AI
- GCP
tags: ["Google Cloud", "GCE", "Hermes Agent", "Gemini", "Telegram Bot", "Systemd"]
---

![image-20260502161538962](../images/image-20260502161538962.png)

# 前情提要

在解決了 LINE Bot 的 Vertex AI 遷移後，我開始思考：能不能有一個「更具主動性」且「擁有長期記憶」的 AI 助手？這時候我盯上了 [NousResearch 開源的 **Hermes Agent**](https://github.com/nousresearch/hermes-agent) 。

不同於一般的 Chatbot，Hermes 被設計成一個「會呼吸的操作系統」，它能自己執行 Shell 指令、寫 Python 腳本、管理長期記憶，甚至能透過不同的 Gateway (Telegram, Discord) 隨時與你保持聯繫。

為了讓它能 24/7 待命，我選擇將它部署在 **Google Compute Engine (GCE)** 上。這篇文章將紀錄從零開始的部署過程，以及我在配置最新的 **Gemini 2.5 Flash** 模型時踩過的那些坑。

---

## 環境參數預備

在開始之前，請確保你手上有這些必要的參數：
*   **PROJECT_ID**: `YOUR_PROJECT_ID`
*   **LOCATION**: `global`
*   **GOOGLE_API_KEY**: `YOUR_GOOGLE_API_KEY` (Google AI Studio 取得)

---

## 第一步：建立 GCE 實例

Hermes Agent 需要一點運算能力來處理工具調用（Tool Use），建議使用 `e2-medium` 規格。

```bash
gcloud compute instances create hermes-agent-vm \
    --project=YOUR_PROJECT_ID \
    --zone=us-central1-a \
    --machine-type=e2-medium \
    --image-family=ubuntu-2204-lts \
    --image-project=ubuntu-os-cloud \
    --boot-disk-size=30GB \
    --metadata=startup-script='#!/bin/bash
        apt-get update
        apt-get install -y git curl python3-pip python3-venv nodejs npm
    '
```

---

## 第二步：安裝 Hermes Agent

SSH 進入 VM 後，直接使用官方提供的一鍵安裝腳本。

1.  **進入 VM**：
    ```bash
    gcloud compute ssh hermes-agent-vm --zone=us-central1-a
    ```

2.  **執行安裝**：
    ```bash
    curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
    source ~/.bashrc
    ```

---

## 第三步：配置 Gemini 2.5 Flash (SOP 實戰)

這是整場演習最容易踩坑的地方。Hermes 預設可能會指向不存在或過期的模型標識符。

1.  **建立配置文件**：
    在 `~/.hermes/config.yaml` 中，我們必須精確指定 **Gemini 2.5 Flash**，且 **不可帶 `google/` 前綴**。

    ```yaml
    model:
      provider: gemini
      default: "gemini-2.5-flash"
    terminal:
      backend: local
    gateway:
      provider: telegram
    auxiliary:
      # ⚠️ 非常重要：Hermes 內部會硬編碼輔助模型，必須手動覆寫
      title_generation: { provider: gemini, model: "gemini-2.5-flash" }
      summarization: { provider: gemini, model: "gemini-2.5-flash" }
    ```

2.  **設定 API Key**：
    在 `~/.hermes/.env` 中寫入密鑰與權限設定：
    ```bash
    echo "GOOGLE_API_KEY=你的_API_KEY" >> ~/.hermes/.env
    echo "GATEWAY_ALLOW_ALL_USERS=true" >> ~/.hermes/.env
    ```

---

## 第四步：接軌 Telegram 與背景持久化

為了不讓 SSH 斷線後 Agent 就消失，我們使用 **Systemd** 來管理。

1.  **建立 Systemd 服務** (`/etc/systemd/system/hermes.service`)：
    ```ini
    [Unit]
    Description=Hermes Agent Gateway
    After=network.target
    
    [Service]
    Type=simple
    User=root
    Environment=HOME=/root
    Environment=PYTHONUNBUFFERED=1
    ExecStart=/usr/local/lib/hermes-agent/venv/bin/hermes gateway run
    Restart=always
    RestartSec=10
    
    [Install]
    WantedBy=multi-user.target
    ```

2.  **啟動服務**：
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable hermes
    sudo systemctl restart hermes
    ```

---

## 遷移過程中的血淚踩坑：為什麼我的 Agent 沒反應？

即使配置正確，我還是遇到了「Agent 讀取訊息但死不回覆」的窘境。查了日誌 (`journalctl -u hermes`) 才發現幾個深坑：

### 踩坑一：Gemini 3.0 的 404 幽靈

我在配置時試圖追求最新，用了 `gemini-3-flash-preview`。結果日誌噴出一堆 **404 Model Not Found**。
**原因**：Hermes 內部的 `auxiliary_client.py` 硬編碼了許多 `gemini-3-flash-preview` 作為預設值。當這些輔助功能（如生成標題）報錯時，會連帶影響整個 Gateway 的回覆邏輯。
**解法**：手動在 `config.yaml` 中顯式定義所有 `auxiliary` 模型為 `gemini-2.5-flash`，或者直接用 `sed` 去補丁源代碼。

### 踩坑二：模型標識符的前綴混淆

在不同的 SDK 中，有人用 `google/gemini-2.5-flash`，有人用 `gemini-2.5-flash`。
**經驗**：在 Hermes 的 Gemini Provider 中，**直接使用短名稱 `gemini-2.5-flash` 是最保險的**。加了 `google/` 反而會讓 API 路由出錯。

### 踩坑三：Systemd 與「已經運行的進程」衝突

當你手動跑過 `hermes gateway` 後再啟動服務，系統會報 `Gateway already running (PID xxxx)`。
**解法**：在 Systemd 的 `ExecStart` 之前，可以加一條 `ExecStartPre=/usr/bin/pkill -9 -f hermes || true`，確保每次啟動都是乾淨的環境。

---

## 總結

現在，我的專屬 Hermes Agent 已經穩定的跑在 GCE 上，並且透過 Telegram 隨時待命。它不僅能幫我查資料，還能直接在雲端 VM 上幫我跑一些簡單的運算腳本。

這次部署讓我學到：**面對更新極快的模型，官方文檔（或 MCP 工具查詢）才是唯一的真理**。不要盲目追求最新版本號，確保標識符與當前 API 環境匹配才是穩定運行的關鍵。

如果你也想要一個 24 小時不睡覺的 AI 數位替身，趕快照著這份 SOP 弄一台吧！
