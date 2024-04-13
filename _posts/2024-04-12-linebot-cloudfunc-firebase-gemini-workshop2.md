---
layout: post
title: "[BwAI workshop][Golang] LINE OA + CloudFunction + GeminiPro + Firebase = 旅行小幫手 LINE 聊天機器人(2)： Firebase Database 讓 LINEBot 有個超長記憶"
description: ""
category: 
- Python 
- TIL
tags: ["Golang", "LINEBot", "Firebase", "GoogleCloud", "CloudFunction"]
---

<img src="../images/2022/image-20240413210750427.png" alt="image-20240413210750427" style="zoom:67%;" />

# 前言:

這是一篇為了 04/18 跟 Google Developer Group 合作的 BUILD WITH AI (BWAI) WORKSHOP 的第二篇系列文章（不知道還需要幾篇）。

本篇文章將專注在以下幾個部分：

- Firebase Database 設定
- 如何在 Cloud Function 上透過官方 Golang 存取 Firebase
- 透過 Firebase Database 來讓你的 Gemini 記住所有講過的事情，優化[上一次](https://www.evanlin.com/linebot-cloudfunc-firebase-gemini-workshop/)打造的 LINE Bot



# 文章列表：

-  [[BwAI workshop][Golang] LINE OA + CloudFunction + GeminiPro + Firebase = 旅行小幫手 LINE 聊天機器人(一)： 景色辨識小幫手](https://www.evanlin.com/linebot-cloudfunc-firebase-gemini-workshop/)
-  [[BwAI workshop][Golang] LINE OA + CloudFunction + GeminiPro + Firebase = 旅行小幫手 LINE 聊天機器人(2)： Firebase Database 讓 LINEBot 有個超長記憶]()


# 事前準備

- **[LINE Developer Account](https://developers.line.biz/en/)**: 你只需要有 LINE 帳號就可以申請開發者帳號。
- [**Google Cloud Functions**](https://cloud.google.com/functions?hl=zh_cn)： ＧGo 程式碼的**部署平台**，生成供 LINEBot 使用的 webhook address。
- [**Firebase**](https://firebase.google.com/)：建立**Realtime database**，LINE Bot 可以記得你之前的對話，甚至可以回答許多有趣的問題。
- **[Google AI Studio](https://aistudio.google.com/)**:可以透過這裡取得 Gemini Key 。



## 申請 Firebase Database 服務



## 申請 Services Account Credential 讓你的 Cloud Function 連接 Google 服務





#  完整原始碼

你可以在這裡找到相關的開源程式碼: [https://github.com/kkdai/linebot-cf-firebase]( https://github.com/kkdai/linebot-cf-firebase)





