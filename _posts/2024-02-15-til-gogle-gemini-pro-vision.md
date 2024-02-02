---
layout: post
title: "[Golang][Gemini Pro] 使用 Gemini-Pro-Vision 來打造名片管理的聊天機器人"
description: ""
category: 
- Golang
tags: ["Golang", "GoogleGemini", "LLM"]

---

![img](../images/2022/add_card.jpg)





# 前提

前幾篇的文章 曾經提過，如何整合基本的 Gemini Pro Chat 與 Gemini Pro Vision 兩個模型的使用。 本次將快速提一下，打造一個具有記憶體的 LINE Bot 該如何做。

##### 相關開源程式碼：

[https://github.com/kkdai/linebot-smart-namecard](https://github.com/kkdai/linebot-smart-namecard)



## 系列文章：

1. [使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (一）: Chat Completion and Image Vision](https://www.evanlin.com/til-gogle-gemini-pro-linebot/)
2. [使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (二）: 使用 Chat Session 與 LINEBot 快速整合出有記憶的 LINE Bot ](https://www.evanlin.com/til-gogle-gemini-pro-chat-session/)
3. 使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (三）: 使用 Gemini-Pro-Vision 來打造名片管理的聊天機器人 (本篇)

## 目前 Gemini Pro 的收費

截至筆者寫完（2024/01/03) 目前的定價依舊是 (refer [Google AI Price](https://ai.google.dev/pricing))

- 一分鐘內 60次詢問都免費
- 超過的話:
  - $0.00025 / 1K characters
  - $0.0025 / image

<img src="../images/2022/image-20240103223633970.png" alt="image-20240103223633970" style="zoom:50%;" />

## 透過 Render 來快速部署服務:

由於 Gemini Pro 目前在某些額度下，還是免費的。這裡也更改了專案，讓沒有信用卡的學生可以學習如何打造一個 LLM 具有記憶的聊天機器人。

### Render.com 簡介：

- 類似 Heroku 的 PaaS (Platform As A Services) 服務提供者。
- 他有免費的 Free Tier ，適合工程師開發 Hobby Project。
- 不需要綁定信用卡，就可以部署服務。

參考： [Render.com Price](https://render.com/pricing)



### 部署步驟如下：

- 到專案頁面 [kkdai/linebot-gemini-pro: LINE Bot sample code how to use Google Gemini Pro in GO (Golang) (github.com)](https://github.com/kkdai/linebot-gemini-pro)
- 按下 Deploy To Render

![image-20240104140246518](../images/2022/image-20240104140246518.png)

- 選擇一個服務名字

![image-20240104140347932](../images/2022/image-20240104140347932.png)

- 這邊有三個需要填寫：
  - **ChannelAccessToken**: 請到 [LINE Developer Console](https://developers.line.biz/console/) 取得。
  - **ChannelSecret**: 請到 [LINE Developer Console](https://developers.line.biz/console/) 取得。
  - **GOOGLE_GEMINI_API_KEY**: 請到 [Google AI Studio](https://makersuite.google.com/app/apikey) 取得。
- 這樣就部署成功了，記得還要跟 LINE Bot 串接起來。

## 成果





# 參考資料：

- [OpenAI ChatCompletion API](https://platform.openai.com/docs/guides/text-generation/chat-completions-api)
- [google.generativeai.ChatSession](https://ai.google.dev/api/python/google/generativeai/ChatSession?hl=en)
- [Google AI Studio API Price](https://ai.google.dev/pricing)
- [GoDoc ChatSession Example](https://pkg.go.dev/github.com/google/generative-ai-go/genai#example-ChatSession)
- [Google GenerativeAI ChatSession Python Client](https://ai.google.dev/api/python/google/generativeai/ChatSession?hl=en) 
