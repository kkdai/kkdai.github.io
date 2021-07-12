---
layout: post
title: "[學習心得] LINE Bot 開發者指南 詳解 - 5. LINE Login (補充)"
description: ""
category: 
- 學習文件
tags: ["LINEBot", "Chatbot", "DevRel"]
---

<img src="../images/2021/linebot005.jpg">

## 前言:

各位好， 我是 LINE Taiwan 資深開發技術推廣工程師 – Evan Lin。 今天這篇文章為各位詳細解釋 「 LINE Bot 開發指南」這一份投影片文件。這一份文件是來自於 [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/) 的投影片，考量到在台灣還沒有正式的公布與中文化。這一次跟總部共同合作準備中文版本之外，並且特定用這一系列文章加以解釋，希望可以讓更多開發者有更多的了解。  [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/)  文件內容很多，本份投影片也將以五篇文章的篇幅來加以解釋。本篇文章為第四篇文章，主要講解的會是關於 LINE Login 與開發時候需要注意的事項。



## 文章索引:

#### 完整投影片鏈結： <https://speakerdeck.com/line_developers_tw2/line-bot-developer-guideline-chinese>

希望各位可以持續關注：

1. [關於LINE Bot ](https://www.evanlin.com/2021-05-25-line-bot-guide-1/)
2. [使用Webhook URL接收請求時的注意事項](https://www.evanlin.com/line-bot-guide-2/)
3. [發送 API 請求時的注意事項](http://www.evanlin.com/line-bot-guide-3/)
4. [LINE Login (本篇文章)](http://www.evanlin.com/line-bot-guide-4/)
5. [LINE Login (補充）(本篇文章)](http://www.evanlin.com/line-bot-guide-5/)
5.  其他相關功能

本篇文章將專注在第一個段落，也就是 Page 47 ~ Page 30 的部分。

##  LINE Login (補充)

<script async class="speakerdeck-embed" data-slide="47" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

本篇注意事項中，將會帶出以下的相關項目。

- 關於防止以 state 不法使用的對策

- 依照不同流程 OS 進行 LINE Login 的用戶流程範例 (iOS)
- 依照不同流程 OS 進行 LINE Login 的用戶流程範例 (Android)
- 外部瀏覽器的登入流程（示意圖）
- 關於轉換目標端瀏覽器的設定方法

以下開始將會逐一針對每一個頁面詳細解釋：

## 關於防止以 state 不法使用的對策

<script async class="speakerdeck-embed" data-slide="48" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這邊主要是提到 state 參數的使用方式， 詳細的步驟可以參考教學部落格 [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/) 。這邊也有列出在官方文章中的使用

![img](https://developers.line.biz/assets/img/web-login-flow.2af66354.svg)





#### 參考文章:

- 

## 依照不同流程 OS 進行 LINE Login 的用戶流程範例 (iOS)

<script async class="speakerdeck-embed" data-slide="49" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>




## 依照不同流程 OS 進行 LINE Login 的用戶流程範例 (Android)

<script async class="speakerdeck-embed" data-slide="49" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>


#### 相關文件：

-  

## 外部瀏覽器的登入流程（示意圖）

<script async class="speakerdeck-embed" data-slide="50" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



#### 相關文件：

-  

## 關於轉換目標端瀏覽器的設定方法

<script async class="speakerdeck-embed" data-slide="51" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



#### 相關文件

- 



## 結論：

<a id="summary"></a>

以上就是「LINE Bot 開發指南」第四部分的補充與分享，想要知道更多內容可以查看完整投影片，或是找到其他篇的文章來了解。 

想了解更多開發者的活動？  立即加入「LINE 開發者官方社群」官方帳號，就能收到第一手 Meetup 活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE 開發者官方社群」官方帳號 ID：@line_tw_dev
![img](https://www.evanlin.com/images/2020/line-tw-dev-qr.png)

