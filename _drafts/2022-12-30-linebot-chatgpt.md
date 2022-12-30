---
layout: post
title: "[學習文件] LINE Bot 群組聊天摘要生成器"
description: ""
category: 
- 學習文件
tags: ["LINEBot", "Golang", "ChatGPT"]
---



![image-20221230150344227](../images/2022/image-20221230150344227.png)



# 前言:

大家好，我是 LINE 台灣的資深技術推廣工程師的 Evan Lin 。前一段時間， [OpenAI](https://openai.com) 將他們知名的 GPTv3 的 NLP Model 開放出來給大家使用，並且提供一個好用的介面 [ChatGPT](https://chat.openai.com/chat) 。 在全世界受到相當多的注意，也看到有相當多社群的開發者有分享相關的開發案例。

- [我嘗試將 GPT-3 整合到 LINE 聊天機器人中](https://dev.classmethod.jp/articles/chatgpt-line-chat-bot/)
- https://beta.openai.com/docs/api-reference/completions/create
- https://github.com/isdaviddong/chatGPTLineBot
- [https://github.com/memochou1993/gpt-ai-assistant](https://github.com/memochou1993/gpt-ai-assistant)

但是其實 ChatGPT 不是直接串接到聊天機器人就好，或許他也可以幫助我們解決許多以前的痛點。 以下這篇文章將分享如何透過  [ChatGPT](https://chat.openai.com/chat) 來打造出一個專門在群組間幫你做摘要的聊天機器人。



# 原始碼：

#### [https://github.com/kkdai/LINE-Bot-ChatSummarizer](https://github.com/kkdai/LINE-Bot-ChatSummarizer)



# 解決的問題痛點







# 如何用？如何架設？ 

# 如何安裝

### 獲取 LINE Bot API 開發者帳戶

- 如果你想使用 LINE Bot，請確保在 [https://developers.line.biz/console/]( https://developers.line.biz/console/) 註冊 LINE 開發者控制台的帳號。
- 在「基本設定」選項卡上創建新的消息通道並獲取「Channel Secret」。
- 在「Messiging API」選項卡上獲取「Channel Access Token」。
- 從「基本設定」選項卡中打開 LINE OA 管理器，然後轉到 OA 管理器的回復設定。在那裡啟用「webhook」。

### 獲取 OpenAI API Token

- 在 https://openai.com/api/ 註冊帳戶。
- 一旦你有了帳戶，就可以在帳戶設定頁面找到你的 API 權杖。
- 如果你想在開發中使用 OpenAI API，你可以在 API 文檔頁面中找到更多信息和說明。

請注意，OpenAI API 只面向滿足某些條件的用戶開放。你可以在 API 文檔頁面中找到有關 API 的使用條件和限制的更多信息。

### 部署在 Heroku 上

- 輸入「Channel Secret」、「Channel Access Token」和「ChatGPT Access Token」。

- 記住你的 Heroku 伺服器 ID。

### 在 LINE Bot Dashboard 中設置基本 API：

- 設置你的基本帳戶信息，包括「回調 URL」在 [https://{YOUR_HEROKU_SERVER_ID}.herokuapp.com/callback](https://{your_heroku_server_id}.herokuapp.com/callback)。



# 成果

- 依照「如何安裝」進行相關的部署流程。
- 將機器人加入群組聊天室。

[![img](../images/2022/chat_1.png)](https://github.com/kkdai/LINE-Bot-ChatSummarizer/blob/master/img/chat_1.png)

- 能夠記住群組內的對話。

![list_all.png](../images/2022/list_all-20221230151706913.png)



- 直接送群組內容摘要私訊給你。

![sum_all.png](../images/2022/sum_all-20221230151647428.png)



### 相關指令如下

- `:gpt xxx`: 直接對 ChatGPT 來對談，可以直接問他。
- `:list_all`: 列出群組的訊息紀錄（all)
- `:sum_all`: 幫你做訊息摘要。



# 開發流程記錄：








## 相關技術文件：

- [LINE API: Getting user profiles](https://developers.line.biz/en/docs/android-sdk/managing-users/#get-profile)
- [LINE API: Get group chat member profile](https://developers.line.biz/en/reference/messaging-api/#get-group-member-profile)
- [LINE API: Get group chat summary](https://developers.line.biz/en/reference/messaging-api/#group)


# 未來相關工作



如果你有任何建議或是疑問，歡迎透過 [LINE Developers 的官方討論區](https://www.facebook.com/groups/linebot)或是[LINE 開發者官方社群的官方帳號跟我們聯絡](https://lin.ee/qZRsSTG)。


# LINE Developers 相關技術文件與部落格：  

- [[新聞\] Flex Message Update 3 released](https://developers.line.biz/en/news/2022/03/11/flex-message-update-3-released/)

-  [文件: Creating a Flex Message including a video](https://developers.line.biz/en/docs/messaging-api/create-flex-message-including-video/)

- [部落格: 2022 年 Flex Message 的 3 項新功能 LINE 中訊息設計釋放無限自由](https://engineering.linecorp.com/zh-hant/blog/2022-flex-message-v3/)

  

   

