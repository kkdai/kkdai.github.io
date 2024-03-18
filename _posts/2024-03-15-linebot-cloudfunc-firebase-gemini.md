---
layout: post
title: "[LINE Bot][Python] Cloud Function + Gemini Pro + Firebase Database = 記憶體聊天機器人"
description: ""
category: 
- Python
- TIL
tags: ["Python", "LINEBot", "Firebase", "GoogleCloud", "CloudFunction"]

---

<img src="../images/2022/image-20240318204441270.png" alt="image-20240318204441270" style="zoom:50%;" />

# 起因

這邊文章，主要是透過[【LineBot實作】如何製作有記憶的對話機器人](https://medium.com/@pearl3904/linebot%E5%AF%A6%E4%BD%9C-%E5%A6%82%E4%BD%95%E8%A3%BD%E4%BD%9C%E6%9C%89%E8%A8%98%E6%86%B6%E7%9A%84%E5%B0%8D%E8%A9%B1%E6%A9%9F%E5%99%A8%E4%BA%BA-0a80a9601e3d) 的相關修改。 把需要付費的服務 OpenAI 改成目前還有免費額度的 Google Gemini ，並且針對相關訊息的程式碼做一些調整。 主要的 LINe Bot 設定與 Firebase 設定請參考原先文章。



# 快速講解一下設定的方法：

這裡僅快速條列式，講解一下該如何設定這個應用。

- 建立 LINE Bot 帳號
  - 首先先去 [LINE Developer Console](https://developers.line.biz/console/?status=success) 建立一個 Messaging API Channel
  - 在 Basic Setting Tab 取得 Channel secret
  - 在 Messaging API Tab 產生 (issue) Channel access token
- 到 Google Cloud 的 [Firebase](https://firebase.google.com/) 
  - 建立一個專案
  - 設定一個 Realtime Database
  - 更改安全規則，讓資料庫可以被任何人存取與修改。（請注意： 這是因為做測試專案，請勿再正式專案如此設定）
  - 取得該資料庫網址： **https://XXX.firebaseio.com/**
- 到 Google Cloud 的 [Cloud Functions](https://cloud.google.com/functions?hl=zh_cn) 
  - 建立一個 Cloud Functions - Gen1 或是 Gen2 都可以
  - 採取 HTTPS
  - 驗證部分： 使用「允許未經驗證的叫用」
  - **新增四個環境變數**：
    1. GEMINI_API_KEY：(在 Google AI Studio 獲得的secret key)
    2. LINE_BOT_TOKEN：(在Line Developers獲得的Channel access token)
    3. LINE_BOT_SECRET：(在Line Developers獲得的Channel secret)
    4. FIREBASE_URL：(在Firebase獲得的URL)
  - 程式碼部分，請使用以下修改的程式碼。

# 主要修改程式碼部分

 首先是 `requirements.txt` 需要將相關設備改成 Gemini pro - `google.generativeai`。

<script src="https://gist.github.com/kkdai/148f57c651f369e771bfd0d86c585563.js"></script>

接下來建立另外一個檔案： `main.py`

<script src="https://gist.github.com/kkdai/a59c50a63f568299c46c013461e15d81.js"></script>

設定 "進入點" 到 `linebot`，並且記錄觸發網址 `https://xxxxxxxx.cloudfunctions.net/function-test1` 。

# 回頭設定 LINE Bot

- 設定  [LINE Developer Console](https://developers.line.biz/console/?status=success)  到了 Messaging API Tab 並且將 Webhook URL 放入剛剛輸入的  `https://xxxxxxxx.cloudfunctions.net/function-test1` 。
- 測試是否成功，即可開始對聊天機器人溝通。

## 參考文章：

- [【LineBot實作】如何製作有記憶的對話機器人](https://medium.com/@pearl3904/linebot%E5%AF%A6%E4%BD%9C-%E5%A6%82%E4%BD%95%E8%A3%BD%E4%BD%9C%E6%9C%89%E8%A8%98%E6%86%B6%E7%9A%84%E5%B0%8D%E8%A9%B1%E6%A9%9F%E5%99%A8%E4%BA%BA-0a80a9601e3d)

