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

## (3) 重新導向後的資料處理：

<script async class="speakerdeck-embed" data-slide="35" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這篇投影片提到了，當使用者輸入完帳號跟密碼完成 LINE Login 註冊之後。 LINE 平台會將結果發送到 Callback URL 所登記的伺服器。這時候會收到相關的資訊需要做後續的處理。

![](https://developers.line.biz/assets/img/web-login-flow.2af66354.svg)
這時候會有兩種方式可以取得，也就是接下來兩頁投影片的方式：

- 取得 Access Token API
- 取得 ID Token 

此外，如果在手機端開發 LINE Login 請記得要使用 PKCE 的呼叫認證方式。讓您的安全性更加全面。  細節可以參考 [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/) 或是技術文件  [PKCE support for LINE Login](https://developers.line.biz/en/docs/line-login/integrate-pkce/)

#### 相關文章

- [Dev Doc: LINE Login: Receiving the authorization code](https://developers.line.biz/en/docs/line-login/integrate-line-login/#receiving-the-authorization-code)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)
- [PKCE support for LINE Login](https://developers.line.biz/en/docs/line-login/integrate-pkce/)

## (4) 取得 access token API 
<script async class="speakerdeck-embed" data-slide="36" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這邊討論取得 Access Token API 呼叫需要注意的參數，相關文章介紹一樣可以參考 [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/) ，也可以查看技術文件  [Getting an access token with a web app](https://developers.line.biz/en/docs/line-login/integrate-line-login/#get-access-token) 。

#### 相關文章

- [Dev Doc: LINE Login : Get Access Token](https://developers.line.biz/en/docs/line-login/integrate-line-login/#get-access-token)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)
-   [Getting an access token with a web app](https://developers.line.biz/en/docs/line-login/integrate-line-login/#get-access-token)

## (5) 取得 ID Token 
<script async class="speakerdeck-embed" data-slide="37" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

取得 ID Token 的前提是必須使用 `scope` 為 `openid` ，才能在收到 response 的時候收到。收到 `id_token` 後有兩個方式可以取得使用者資訊：

- **使用 ID Token 的 Verify API**: 參考 [Use a LINE Login API endpoint](https://developers.line.biz/en/docs/line-login/verify-id-token/)。
- **直接透過程式碼來解析** `id_token`： 參考 [Write code to validate ID tokens](https://developers.line.biz/en/docs/line-login/verify-id-token/#write-original-code) 。

#### 相關文章

- [Dev Doc: LINE Login : Get Access Token](https://developers.line.biz/en/docs/line-login/integrate-line-login/#get-access-token)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)
-  [Write code to validate ID tokens](https://developers.line.biz/en/docs/line-login/verify-id-token/#write-original-code) 
-  [Use a LINE Login API endpoint](https://developers.line.biz/en/docs/line-login/verify-id-token/)

## (6) 取得用戶資料
<script async class="speakerdeck-embed" data-slide="38" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這邊指的是透過 [Get User Profile](https://developers.line.biz/en/docs/line-login/managing-users/#get-profile) 的方式來取得使用者的資料，但是這邊需要事先取得 Access Token 也就是在 Request Access Token 完成後的資訊才可以。

#### 相關文章

- [Dev Doc: LINE Login : Get Access Token](https://developers.line.biz/en/docs/line-login/integrate-line-login/#get-access-token)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)
- [Write code to validate ID tokens](https://developers.line.biz/en/docs/line-login/verify-id-token/#write-original-code) 

## LINE Login 處理流程
<script async class="speakerdeck-embed" data-slide="39" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這邊也有另外一張圖型顯示一樣的事情，主要就是 "Request Access Token" 之後，有以下兩種狀況:

- 使用 `scope` 為 `openid` ，才能在收到 response 的時候收到，收到 `id_token` ，可以直接取得使用者資訊。
- 如果沒有 `id_token` 可以透過 access token 再去取得 Get User Profile 來取得相關資訊。

#### 相關文章

- [Dev Doc: LINE Login : Get Access Token](https://developers.line.biz/en/docs/line-login/integrate-line-login/#get-access-token)
- [如何透過 Golang 開發 OAuth2 的 PKCE – 以 LINE Login 為例](https://engineering.linecorp.com/zh-hant/blog/pkce-line-login/)
- [Write code to validate ID tokens](https://developers.line.biz/en/docs/line-login/verify-id-token/#write-original-code) 



## 自動加好友功能 (1)

<script async class="speakerdeck-embed" data-slide="42" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

在建立 LINE Login Channel 的時候，當初的設定「將 Bot 加入好友選項」有啟動的話，可以將官方帳號一起加入成好友。 本篇投影提供更多相關的參考資料：

#### 【必要的設定】

•要啟用將Bot加成好友的選項時，必須執行「將Bot連結到LINE Login channel」的作業。

#### 【啟用選項的要件】

- 具有被授予LINE Login使用權限（「WEB」或「NATIVE_APP」）的channel。
- 至少存在一個以上的Bot，Bot的Messaging API channel和LINE Login channel是隸屬同Provider。
- 進行設定的用戶，具有在**LINE Official Account Manager關聯****的官方帳號Admin權限**。**
- **進行設定的用戶，具有在**LINE Developers中的** **Login** **channel Admin權限**。

#### 【注意事項】

- 登入時選擇加成好友則和官方帳號關係將被更新。
- 原則上在選擇官方帳號時，除了該channel的正式官方帳號以外，禁止與其餘官方帳號建立關聯性。
- 由於更新會立即反映，因此為了避免誤設到其他帳號（測試帳號等），請在操作時特別小心。
- 如果LINE Login channel是建立在Certified Provider之下，則預設值將會是已勾選加成好友（解除封鎖）的狀態。


#### 【關於 bot_prompt 參數的介紹】

- `normal`: 透過設定方式，提供使用者自己選擇是否要加入官方帳號為好友。
- `aggresive`: 比較積極的方式，透過多彈出一個視窗，可以讓使用者來加入好友。（如果前一個頁面沒有加入的話）。

![](https://developers.line.biz/assets/img/bot-prompt.1dd4e585.png)

## 好友狀態檢查 API 
<script async class="speakerdeck-embed" data-slide="46" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>




## 結論：

<a id="summary"></a>

以上就是「LINE Bot 開發指南」第四部分的補充與分享，想要知道更多內容可以查看完整投影片，或是找到其他篇的文章來了解。 

想了解更多開發者的活動？  立即加入「LINE 開發者官方社群」官方帳號，就能收到第一手 Meetup 活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE 開發者官方社群」官方帳號 ID：@line_tw_dev
![img](https://www.evanlin.com/images/2020/line-tw-dev-qr.png)

