---
layout: post
title: "[iThome Cloud Summit 2023] 結合生成式 AI 打造有趣的 LINE Bot 應用"
description: ""
category: 
- 研討會心得
tags: ["研討會心得", "LINE"]
---

<script defer class="speakerdeck-embed" data-slide="17" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

# 前言

大家好，我是 LINE 台灣的資深技術推廣工程師的 Evan Lin 。前一段時間， [OpenAI](https://openai.com/) 將他們知名的 GPTv3 的 NLP Model 開放出來給大家使用，並且提供一個好用的介面 [ChatGPT](https://chat.openai.com/chat) 。 在全世界受到相當多的注意，也看到有相當多社群的開發者有分享相關的開發案例。這一次我在 iThome 舉辦的 Cloud Summit 2023 擔任講者，並且分享「結合生成式 AI 打造有趣的 LINE Bot 應用」。



## 議程簡介

在現今智慧手機普及的時代，LINE 已成為人們溝通的主要管道之一，而 LINE Bot 更是其中一種廣受歡迎的應用方式。本演講將探討如何結合生成式人工智慧技術，打造有趣的 LINE Bot 應用。透過深度學習模型的訓練，我們可以讓 LINE Bot 具有更智能的對話能力，甚至可以幫助使用者解決問題、提供娛樂等多種應用場景。本演講將從實際案例入手，分享如何利用生成式 AI 技術開發 LINE Bot 應用，讓您了解如何運用 AI 技術，打造具有創意及趣味性的智慧應用。



## 投影片

<script defer class="speakerdeck-embed" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

## 生成式 AI 浪潮下社群有許多好玩的應用

這裡分享了幾個有趣的應用，大家可以參考看看：

- **歷史名人 Bot：** [加入好友](https://lin.ee/OGlQDqj)
  - 還是會真的回覆相關內容，而且很堅持講日文。 XD
- **拼盤小幫手：** [加入好友](https://lin.ee/n8a6OU)
  - 原本是命理相當有研究的社群高手，透過生成式 AI 讓他的內容更加的口語化與人性化。
- **就可妹妹：**[加入好友](https://liff.line.me/1645278921-kWRPP32q)
  - 讓生成式AI 幫你講笑話，不過好像不太好笑ＸＤ
- **Cofacts 真的假的：** [加入好友](https://line.me/R/ti/p/%40279grbvd)
  - 透過生成式AI 快速幫使用者檢查訊息是否是假的機率。

而透過 LINE 的生態系中，有著許多文字的相關應用。有哪一些有趣的應用可能會出現呢？



## 訊息摘要小幫手

<script defer class="speakerdeck-embed" data-slide="11" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

大家是否都有類似的問題？ 常常加入一個群組內，有太多的訊息在裡面跑來跑去，一回頭來看，發現已經有太多未讀的訊息在裡面了。 常常需要進去後，慢慢地追每一個訊息來避免自己錯過太多 (FOMO) ？ 大家可以參考一下[這篇文章](https://engineering.linecorp.com/zh-hant/blog/linebot-chatgpt)如何透過生成式AI 來打造聊天室中的訊息摘要小幫手。

# 透過 LangChain 來打造自主性的 LINE Bot



透過基本的生成式AI 打造一個聊天機器人之後，我們可以進階的透過一些有系統的架構來打造「自主性」的 LINE Bot 。 所謂的自主性會是什麼意思：

- 具有基本思考與處理能力，知道資料要去哪裡尋找。
- 有相關的記憶能力，能記住使用者的相關需求。
- 可以有效的回覆使用者的疑問，或是可以啟動相關的服務。

這裡就要介紹如何透過 [LangChain](https://github.com/hwchase17/langchain) 來打造一個具有「自主性」的 LINE Bot 小幫手。



## 什麼是 LangChain?

<script defer class="speakerdeck-embed" data-slide="17" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



要讓你的 LINE Bot 做出以上的事情，你需要很多很多的相關 Prompt 。不論是定義 LLM 的模型該如何解讀你的文字，該如何挑選即將要執行的動作，或是如何將結果作為有效的拆解，到把以往訊息得內容加以存在 Prompt 之中。

但是透過  [LangChain](https://github.com/hwchase17/langchain)  你可以將這些工作拆解成一個個的小方塊，讓你打造相關服務與應用的時候不用在重複使用那些的 Prompt ，就可以快速打造出來。 以下透過一些簡單的程式碼範例來講解使用了  [LangChain](https://github.com/hwchase17/langchain) 之後，你的程式碼會變得多簡潔。



## 關於如何自主性挑選適合的 Tool 去做的能力 Function Calling

這個部分很建議看一下這篇文章，裡面有相當多的說明。

<script defer class="speakerdeck-embed" data-slide="17" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



透過這個流程的解釋，你可以很了解到為什麼 LLM 可以了解使用者的內容。並且知道要如何去拆解變成參數。這就是如何讓「自主性」這件事情成真的重點。

# LangChain 的程式碼

<script defer class="speakerdeck-embed" data-slide="21" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

根據以上的程式碼，你可以看到定義一個工具的重點，就是要跟 LLM 模型講：

- 這麼工具是做什麼的？(他是一個查詢股價的工具)
  -  `"Useful for when you need to find out the price of stock. You should input the stock ticker used on the yfinance API"`
- 他需要什麼樣的參數？ 你需要給定股價的 ticker
  - `stockticker`就是股票代號，是一個字串。

透過這些資訊 LLM 可以透過剛剛提供的有效思考來幫你查詢股票。那結果會如何呢？

<script defer class="speakerdeck-embed" data-slide="23" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



## 透過打包 LangChain 的小工具 - EmbedChain 打造客服機器人

<script defer class="speakerdeck-embed" data-slide="24" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

[EmbedChain](https://github.com/embedchain/embedchain) 提供一個良好的打包工具，透過 [LangChain](https://github.com/hwchase17/langchain) 為底層打包出相關的文件解讀的套件。他支持的文件有：

- PDF 檔案
- YouTube 的字幕檔案（如果有）
- 網頁
- 甚至是透過程式碼把相關內容丟給他

他就會針對這樣的內容，將其做過 Split 與 Embeding 之後。可以透過一問一答的方式來查詢資料。 我也將相關的資料打包成一個 LINE Bot 的套件，大家可以看一下： 

- 相關範例程式碼： [https://github.com/kkdai/linebot-embedchain](https://github.com/kkdai/linebot-embedchain)
- 相關文章： [https://www.evanlin.com/langchain-embedchain/]( https://www.evanlin.com/langchain-embedchain/)





# 透過生成式 AI 打造 LINE Bot 的小訣竅

以下開始整理一些，你在打造生成式AI的時候可能會需要知道的小訣竅：



## Tip1: 加上相關規則與規範式輸出

<script defer class="speakerdeck-embed" data-slide="30" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這篇可以參考[教學影片（中文)](https://www.youtube.com/playlist?list=PLly8vI0gpqtpTB7mt_qi57qOKQRo4XWAQ)

就像一開始在使用生成式 AI 開發相關的應用一樣，所有的工程團隊都在思考如何透過有效的「咒語」（也就是 Prompt) 跟 LLM 溝通。這邊有相當多的 prompt 可以有效地讓 LLM 盡可能地依照你的要求來做事，並且遇到其他額外的要求（比如說叫他寫一首詩）能避免掉。

要注意 Prompt 使用英文的效果會最好，雖然我們經常使用母語來問答。但是要走 API 的時候，建議還是要用英文。



## Tip2: 教懂你的 LLM

<script defer class="speakerdeck-embed" data-slide="31" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

第二個小訣竅，就是教導大家如何讓你的 LLM 更加的聰明。 雖然我們都知道現在使用的許多 LLM 都是相當的聰明，但是經常還是需要一些敘述來讓他更聰明，能夠有效地處理使用者的疑問。

這裡的案例是使用[中央氣象局的天氣資料 API](https://opendata.cwb.gov.tw/userLogin) 的時候，在輸入地區的時候，由於官方系統規定使用的規定的縣市名稱來輸入。不能接受其他地區的輸入。比如說：墾丁，關西.. 這些不屬於台灣縣市的輸入。 甚至是「台北縣」「台北市」，因為他給的範例只接受「臺北縣」「臺北縣」。 



這時候該怎麼辦呢？ 只要在說明的 Prompt 中去詳細敘述即可，因為其實 Agent 的 說明 'Description' 其實就是 Prompt 。詳細更多內容也可以參考[這篇文章](https://www.evanlin.com/langchain-cwb/)。 



## Tip3: 專注的小幫手

<script defer class="speakerdeck-embed" data-slide="34" data-id="0193e5479d8643efa03b6766e17ac0a3" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

最後給予大家一些小建議，建議儘量不要讓所有功能放在一個聊天機器人上。就像是開發 App 一樣，一個好的 LINE 官方帳號應該是盡可能的滿足使用者在一個方面的需求。 如此一來能將所有使用者體驗加以優化， 比如說：

- 翻譯小幫手
- [股價小幫手](https://github.com/kkdai/linebot-langchain)
- [旅遊小幫手](https://github.com/kkdai/linebot-travel-chatgpt)
- [論文小幫手](https://github.com/kkdai/linebot-arxiv)

# 讓你的 LINE 官方帳號創業夢實現

這裡跟大家分享，在沒有生成式AI 之前，其實許多交談對話的處理上都需要使用到 NLU(自然語言理解) 的技術。但是當時的 NLU 技術其實還有相當大的限制，造成許多的流程無法有效地改善整體使用者的體驗。

透過「生成式AI」的幫助，我們相信許多的流程會變得更加的簡單，不光只是將訊息丟給 LLM 來回答而已。更可以有相關更多的相關應用，比如說：

- 一些行事曆的相關工具類別：
  - 透過 LangChain 的幫助，你可以快速打造一些打字就輸入相關記事的內容。透過將「新增記事」打造成一個工具，來連接到你的資料庫，或是一些雲端的工具來協助紀錄事項。 但是透過 LangChain 的 Agent 技術可以快速地去開發出透過口語化文字可以快速了解使用者的需求，再也不需要透過某一些「關鍵字」的處理方式來處理需求。
- 一些文字導覽型的應用：
  - 透過文字導覽類型的應用，原本無法做成有效的 Q&A 。但是得力於 LangChain 甚至是 EmbedChain 的應用，我們可以將相關文件導入之後。讓生成式AI 聊天機器人來協助回答問題。



最後，生成式 AI 是一個很好的工具。可以讓你的 LINE 官方帳號更加的有溫度，我們也希望每一位開發者都可以嘗試將自己的 LINE 聊天機器人來加入相關的功能，讓我們的使用者與官方帳號能夠更加的「Closing The Distance」。

# LINE Developers 相關技術文件與部落格：

- [[新聞\] Flex Message Update 3 released](https://developers.line.biz/en/news/2022/03/11/flex-message-update-3-released/)
- [文件: Creating a Flex Message including a video](https://developers.line.biz/en/docs/messaging-api/create-flex-message-including-video/)
- [部落格: 2022 年 Flex Message 的 3 項新功能 LINE 中訊息設計釋放無限自由](https://engineering.linecorp.com/zh-hant/blog/2022-flex-message-v3/)

# 活動小結

感謝最後一個議程的每一位聽眾。我相信我可能是前幾位在台灣大場地講 LangChain 的講者。 但是接下來開始自主性聊天機器人(Chat Bot) 或是代理人服務（Agent) 將會是市場上一個很主流的議題。 



希望每一個開發者都能透過生成式AI 打造具有溫度的 LINE 官方帳號，一起來 **Closing the Distance.** 



立即加入「LINE 開發者官方社群」官方帳號，就能收到第一手 Meetup 活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE 開發者官方社群」官方帳號 ID：[@line_tw_dev](https://qr-official.line.me/gs/M_908lugfe_BW.png)

![img](../images/2022/M_908lugfe_BW.png)

# 關於「LINE 開發社群計畫」

LINE 於 2019 年開始在台灣啟動「LINE 開發社群計畫」，將長期投入人力與資源在台灣舉辦對內對外、線上線下的開發者社群聚會、徵才日、開發者大會等，已經舉辦 30 場以上的活動。歡迎讀者們能夠持續回來查看最新的狀況。詳情請看:

- - [2021 年 LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/2021-line-tw-devrel/)
  - [2020 年 LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/2020-line-tw-devrel/)
  - [2019 年 LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019-plan/)
  - [LINE Taiwan Developer Relations 2019 回顧與 2019 開發社群計畫報告](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019/)