---
layout: post
title: "[GCP 實戰] LINE Bot 遷移大作戰：從 AI Studio 轉向 Vertex AI 解決 429 額度危機"
description: "紀錄將 LINE 名片機器人從 Google AI Studio 遷移至 Google Cloud Vertex AI 的完整過程，探討 Workload Identity 設定，並解決模型區域限制與名稱混淆的踩坑經驗。"
category:
- AI
- GCP
tags: ["Google Cloud", "Vertex AI", "LINE Bot", "Python", "Cloud Run"]
---

![image-20260421011411264](https://www.evanlin.com/images/image-20260421011411264.png)

# 前情提要

最近我們部署在 Google Cloud Run 上的 LINE 名片助理機器人 (`linebot-namecard-python`) 突然罷工了。透過 `gcloud logging read` 查了一下日誌，迎面而來的是這個無情的錯誤：

> `google.api_core.exceptions.ResourceExhausted: 429 Your billing account has exceeded its monthly spending cap.`

這是一個慘痛的教訓：我們當初為了快速開發，直接使用了 Google AI Studio 提供的 API Key（走 `google.generativeai` 套件）。隨著流量增加，我們很快就撞上了 **Google AI Studio Tier 1 的 429 限制（Rate Limit / Quota 爆炸）**，結果默默把每月的免費額度給打爆了。

為了徹底解決這個問題，我們決定將所有依賴 AI Studio Gemini API Key 的應用程式，全面轉換成 **Google Cloud Vertex AI**，直接走 GCP 的企業級架構與計費系統。

這篇文章就來分享這次遷移的完整過程，以及途中發現「Vertex AI 的資訊實在太少，導致模型設定常常踩坑」的各種血淚經驗。

---

## 身份驗證升級：推薦使用 Workload Identity

在遷移的過程中，最重要的一環就是身分驗證。過去我們習慣在環境變數塞一個 `GEMINI_API_KEY`，簡單粗暴但充滿資安隱患。

來到 GCP 的世界，很多人第一直覺是去開一個 Service Account（服務帳戶），然後把 JSON 金鑰載下來放進程式裡。**但強烈建議不要走 Service Account 金鑰這條路！**

搭配 `gcloud cli`，其實可以很快速地為 Cloud Run 設定 **Workload Identity**。程式碼中不需要任何金鑰，只要在執行環境中賦予該 Identity 對應的 `Vertex AI User` 權限，Google Cloud SDK 就會自動取得憑證。這樣不僅大幅降低金鑰外洩的風險，管理起來也更輕鬆。

---

## 程式碼升級：從 AI Studio 轉向 Vertex AI

要將專案從 Google AI Studio SDK 遷移到 Vertex AI，主要有三個步驟：

1. **替換依賴套件**：
   在 `requirements.txt` 中，移除舊的 `google.generativeai`，換成 `google-cloud-aiplatform`。

2. **更新環境變數設定**：
   在 `config.py` 中，我們不再需要 API Key，而是改用 GCP 的 `PROJECT_ID` 和 `LOCATION`：
   ```python
   PROJECT_ID = os.getenv("PROJECT_ID", None)
   LOCATION = os.getenv("LOCATION", "global") # 預設使用 global
   ```

3. **核心程式碼改寫 (gemini_utils.py)**：
   Vertex AI 的 SDK 介面雖然類似，但對於多模態（如圖片）的處理稍微嚴格一點。我們需要將 `PIL.Image` 轉換成 `vertexai.generative_models.Part` 格式：

   ```python
   import vertexai
   from vertexai.generative_models import GenerativeModel, Part
   from io import BytesIO
   import PIL.Image
   
   # 初始化 Vertex AI
   vertexai.init(project=PROJECT_ID, location=LOCATION)
   
   def pil_to_bytes(img: PIL.Image.Image) -> bytes:
       img_byte_arr = BytesIO()
       img.save(img_byte_arr, format='JPEG')
       return img_byte_arr.getvalue()
   
   def generate_json_from_image(img: PIL.Image.Image, prompt: str) -> object:
       model = GenerativeModel(
           "gemini-3-flash-preview",
           generation_config={"response_mime_type": "application/json"},
       )
       # ⚠️ 注意這裡：必須轉換成 Part 物件
       img_part = Part.from_data(data=pil_to_bytes(img), mime_type="image/jpeg")
       response = model.generate_content([prompt, img_part], stream=False)
       return response
   ```

---

## 遷移過程中的血淚踩坑：模型都很笨？還是 Vertex AI 資訊太少？

在遷移的過程中，有一種強烈的感覺：**「模型好像都很笨（我是說在座的所有 models），還是其實是 Vertex AI 官方給的資訊太少了？」** 

許多在 AI Studio 上理所當然的設定，搬到 Vertex AI 後卻頻頻報錯。以下整理了幾個最容易踩到、讓人來來回回除錯的深坑：

### 踩坑一：殘留的舊 SDK 導致 Cloud Run 啟動失敗

滿心歡喜地用 `gcloud run services update` 更新了環境變數，結果 Cloud Run 部署失敗，容器連啟動都啟動不了。

查了日誌才發現：
> `ModuleNotFoundError: No module named 'google.generativeai'`

**原因**：雖然 `gemini_utils.py` 已經改寫好了，但主程式 `app/main.py` 裡面還殘留著 `import google.generativeai as genai` 以及 `genai.configure(...)` 的初始化程式碼。既然 `requirements.txt` 已經移除了這個套件，容器啟動時自然會找不到模組而崩潰。

**解法**：全面 grep 專案，徹底移除所有舊版 SDK 的引用，然後使用 Cloud Build 重新打包 Docker image 再次推送。

### 踩坑二：Region 必須選 global 才有最多跟最新模型

![Google Chrome 2026-04-21 01.12.46](https://www.evanlin.com/images/Google%20Chrome%202026-04-21%2001.12.46.png)

程式碼清乾淨、容器也順利啟動了，但當我在 LINE 傳送圖片時，機器人卻拋出了 500 錯誤。日誌顯示：

> `google.api_core.exceptions.NotFound: 404 Publisher Model ... was not found or your project does not have access to it.`

這是我這次遇到最大的坑！很多人（包含我）直覺上會把 Vertex AI 的 Region 設定跟 Project 或 Cloud Run 的 Region 綁在一起（例如設定為 `asia-east1` 台灣區）。

但在 Vertex AI 的世界裡，**Region 很容易跟 Project Region 搞混**。如果你想要使用最新、最完整的模型（例如 `gemini-3-flash-preview`），你**必須將 Vertex AI 的 LOCATION 設定為 `global`**。如果硬是設定成 `asia-east1` 或 `us-central1`，常常會遇到 404 找不到模型的窘境。

### 踩坑三：Model 名稱選錯，造成來來回回錯誤

在 Google AI Studio，你可以很隨意地用 `gemini-1.5-flash` 這個 alias 甚至省略後綴。但在 Vertex AI，模型名稱的規定非常嚴格且混亂。

如果你名稱選錯（例如少加了 `-002`，或是預覽版名稱拼錯），API 不是直接報錯，就是默默地 Fallback 跑到舊版的 `1.5` 模型去執行，導致你覺得「這模型怎麼變笨了？」，結果查了半天才發現根本叫錯了模型。

**最終解法與建議**：
1. 將 `config.py` 的預設 region 改為 `global`。
2. 呼叫 `vertexai.init(project="line-vertex", location="global")`。
3. 確保使用的模型名稱與 GCP 官方文件完全一致，例如直接指定 `gemini-3-flash-preview`。

---

## 總結：Vertex AI 帶來的改變

經過一番折騰，名片機器人終於滿血復活，並且順利升級到最新的 Gemini 3 Flash Preview 模型。從 AI Studio 遷移到 Vertex AI 後，帶來了幾個顯著的好處：

1. **擺脫 Quota 焦慮**：徹底解決 AI Studio Tier 1 的 429 爆炸問題，直接透過 GCP 帳單扣款，適合生產環境。
2. **安全性大躍進**：捨棄了明文 API Key 與 Service Account 金鑰，擁抱 Workload Identity，架構更安全現代化。
3. **穩定性**：享受企業級的 SLA 保障。

雖然一開始覺得 Vertex AI 資訊太少、模型名稱與 Region 設定讓人頭痛，但只要掌握了 `global` region 以及 Workload Identity 這兩個訣竅，後續的維護其實是非常香的。

完整程式碼已更新至 [GitHub](https://github.com/kkdai/linebot-namecard-python)，如果你也有專案正準備從 AI Studio 搬家到 Vertex AI，希望這篇踩坑紀錄能幫你少走一點彎路！