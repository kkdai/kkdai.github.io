---
layout: post
title: "[學習心得] LINE Bot 開發者指南 詳解 - 4. LINE Login"
description: ""
category: 
- 學習文件
tags: ["LINEBot", "Chatbot", "DevRel"]
---

<img src="../images/2021/linebot004.jpg">

## 前言:

各位好， 我是 LINE Taiwan 資深開發技術推廣工程師 – Evan Lin。 今天這篇文章為各位詳細解釋 「 LINE Bot 開發指南」這一份投影片文件。這一份文件是來自於 [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/) 的投影片，考量到在台灣還沒有正式的公布與中文化。這一次跟總部共同合作準備中文版本之外，並且特定用這一系列文章加以解釋，希望可以讓更多開發者有更多的了解。  [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/)  文件內容很多，本份投影片也將以五篇文章的篇幅來加以解釋。本篇文章為第四篇文章，主要講解的會是關於 LINE Login 與開發時候需要注意的事項。



## 文章索引:

#### 完整投影片鏈結： <https://speakerdeck.com/line_developers_tw2/line-bot-developer-guideline-chinese>

希望各位可以持續關注：

1. [關於LINE Bot ](https://www.evanlin.com/2021-05-25-line-bot-guide-1/)
2. [使用Webhook URL接收請求時的注意事項](https://www.evanlin.com/line-bot-guide-2/)
3. [發送 API 請求時的注意事項](http://www.evanlin.com/line-bot-guide-3/)
4. [LINE Login (本篇文章)](http://www.evanlin.com/line-bot-guide-4/)
5. LINE Login (補充）
5. 其他相關功能

本篇文章將專注在第一個段落，也就是 Page 30 ~ Page 46 的部分。

##  LINE Login

<script async class="speakerdeck-embed" data-slide="30" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

本篇注意事項中，將會帶出以下的相關項目。

- LINE Login 介紹
- LINE Login authentication
- (1) Callback URL 的設定 
- (2) 驗證與授權
- (3) 重新導向
- (4) 取得 access token API 
- (5) 取得 ID Token
- (6) 取得用戶資料
- LINE Login 處理流程
- 透過 LINE Login 建立帳號關聯性的機制
- 自動加好友功能 (1)
- 自動加好友功能 (2)
- 自動加好友功能 (3)
- 自動加好友功能 (4)
- 好友狀態檢查 API 

以下開始將會逐一針對每一個頁面詳細解釋：

## LINE Login 介紹
<script async class="speakerdeck-embed" data-slide="31" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這一個頁面主要是介紹關於 LINE Login ，關於更詳細的 LINE Login 的簡介部分，可以透過以下的文章來了解：

許多的商業服務都會透過會員機制來提供許多專屬的優惠或是獎勵活動，但是會員的註冊與登入流程常常讓許多使用者覺得為難。除了要填寫許多的資料外，使用者還需要額外記住另外一組的帳號密碼。 LINE 在台灣的佔有率相當的高，並且幾乎每個使用者都有 LINE 的帳戶的狀況下，這時候如果能夠直接使用 LINE 帳戶來註冊與登入網站服務的話是不是相當的方便？

LINE Login 除了提供一個方式來登入之外，也可以提供使用者名稱，大頭照的相關資訊。並且透過 LINE Login 也可以同時讓使用者加入商業服務的 LINE官方帳號，讓使用者更無時無可都可以使用到相關的服務。

#### 相關文章
-[如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)


## LINE Login authentication
<script async class="speakerdeck-embed" data-slide="32" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

而在提到關於 LINE Login 的認證流程詳細解釋，基於 OAuth2 與 OpenID 協議的 LINE Login 不僅僅可以打造一個安全沒有疑慮的登入服務外，還可以幫助網站的開發者快速打造相關的服務。

#### 相關文章
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)

## (1) Callback URL 的設定 
<script async class="speakerdeck-embed" data-slide="33" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

關於 Callback URL 的設定上，這邊有一些相關資訊。

- Callback URL 必須符合 HTTPS 。
- 伺服器本身必須要符合 TLS 1.2 與 1.3 。
- 如果需要支援一個以上的 Callback URL，請記得使用換行來分隔。

#### 相關文章
-  [LINE’s APIs now support TLS 1.3](https://developers.line.biz/en/news/2020/07/01/enabled-tls1.3/)
-  [Updated: TLS 1.0 and TLS 1.1 support by the webhook notification source will be discontinued at the end of January 2021](https://developers.line.biz/en/news/2020/10/06/update-webhook-client-and-root-certificate/)
-  [開發LINE聊天機器人不可不知的十件事](https://engineering.linecorp.com/zh-hant/blog/line-device-10/)



## (2) 驗證與授權
<script async class="speakerdeck-embed" data-slide="34" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

關於 LINE Login 的認證網址，有相當多的相關參數可以使用。大概講幾個比較容易被開發者使用到的參數。

- `prompt`: 透過這個參數可以強制性跳出，使用者同意的簽署頁面。可以了解該 App 目前需要哪些權限。
- `bot_prompt` : 可以將你的 LINE Login App 跟 LINE 官方帳號綁定。也就是使用者登入完成後，可以直接將官方帳號加成好友。

更多的相關使用方式與範例可以參考： [Add a LINE Official Account as a friend when logged in (bot link)](https://developers.line.biz/en/docs/line-login/link-a-bot/).


#### 相關文章

- [Dev Doc: LINE Login: User authentication](https://developers.line.biz/en/docs/line-login/integrate-line-login/#making-an-authorization-request)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)
- [Add a LINE Official Account as a friend when logged in (bot link)](https://developers.line.biz/en/docs/line-login/link-a-bot/).

## (3) 重新導向

<script async class="speakerdeck-embed" data-slide="35" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這篇投影片提到了，當使用者輸入完帳號跟密碼完成 LINE Login 註冊之後。 LINE 平台會將結果發送到 Callback URL 所登記的伺服器。這時候會收到相關的資訊需要做後續的處理。

![](https://developers.line.biz/assets/img/web-login-flow.2af66354.svg)

#### 相關文章

- [Dev Doc: LINE Login: Receiving the authorization code](https://developers.line.biz/en/docs/line-login/integrate-line-login/#receiving-the-authorization-code)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)

## (4) 取得 access token API 
<script async class="speakerdeck-embed" data-slide="36" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

## (5) 取得 ID Token
<script async class="speakerdeck-embed" data-slide="37" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

## (6) 取得用戶資料
<script async class="speakerdeck-embed" data-slide="38" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

## LINE Login 處理流程
<script async class="speakerdeck-embed" data-slide="39" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

## 透過 LINE Login 建立帳號關聯性的機制
<script async class="speakerdeck-embed" data-slide="40" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

<script async class="speakerdeck-embed" data-slide="41" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

## 自動加好友功能 (1)
<script async class="speakerdeck-embed" data-slide="42" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



## 自動加好友功能 (2)
<script async class="speakerdeck-embed" data-slide="43" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



## 自動加好友功能 (3)
<script async class="speakerdeck-embed" data-slide="44" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>



## 自動加好友功能 (4)
<script async class="speakerdeck-embed" data-slide="45" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>


## 好友狀態檢查 API 
<script async class="speakerdeck-embed" data-slide="46" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>




## 結論：

<a id="summary"></a>

以上就是「LINE Bot 開發指南」第四部分的補充與分享，想要知道更多內容可以查看完整投影片，或是找到其他篇的文章來了解。 

想了解更多開發者的活動？  立即加入「LINE 開發者官方社群」官方帳號，就能收到第一手 Meetup 活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE 開發者官方社群」官方帳號 ID：@line_tw_dev
![img](https://www.evanlin.com/images/2020/line-tw-dev-qr.png)
