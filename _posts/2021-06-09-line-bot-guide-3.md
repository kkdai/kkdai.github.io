---
layout: post
title: "[學習心得] LINE Bot 開發者指南 詳解 - 3. 發送 API 請求時的注意事項"
description: ""
category: 
- 學習文件
tags: ["LINEBot", "Chatbot", "DevRel"]
---

<img src="../images/2021/linebot003.jpg">

## 前言:

各位好， 我是 LINE Taiwan 資深開發技術推廣工程師 – Evan Lin。 今天這篇文章為各位詳細解釋 「 LINE Bot 開發指南」這一份投影片文件。這一份文件是來自於 [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/) 的投影片，考量到在台灣還沒有正式的公布與中文化。這一次跟總部共同合作準備中文版本之外，並且特定用這一系列文章加以解釋，希望可以讓更多開發者有更多的了解。  [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/)  文件內容很多，本份投影片也將以五篇文章的篇幅來加以解釋。本篇文章為第三篇文章，主要講解的會是關於發送 API 請求的時候需要注意的事項。



## 文章索引:

#### 完整投影片鏈結： <https://speakerdeck.com/line_developers_tw2/line-bot-developer-guideline-chinese>

希望各位可以持續關注：

1. [關於LINE Bot ](https://www.evanlin.com/2021-05-25-line-bot-guide-1/)
2. [使用Webhook URL接收請求時的注意事項](https://www.evanlin.com/line-bot-guide-2/)
3. [發送 API 請求時的注意事項(本篇文章)](http://www.evanlin.com/line-bot-guide-3/)
4. LINE Login
5.  其他相關功能

本篇文章將專注在第一個段落，也就是 Page 20 ~ Page 30 的部分。

##  發送 API 請求時的注意事項

<script async class="speakerdeck-embed" data-slide="20" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

本篇注意事項中，將會帶出以下的相關項目。

- A. Channel Access Token 的發行

- B. Channel Access Token 自動更新
- C. Channel Access Token 有效上限數量
- D. 訊息發送完成後接收回應
- E. API 請求重試
- F. 請求的相關限制
- G. 回應 ( reply ) 訊息與推播( push )訊息
- H. HTTPS 內容的使用

以下開始將會逐一針對每一個頁面詳細解釋：

## A. Channel Access Token 的發行

<script async class="speakerdeck-embed" data-slide="22" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

Channel Access Token 是整個 Channel 最重要的憑證，透過該憑證可以有許多權限可以修改該 LINE Bot 的設定。所以在授權上要務必小心。 這邊也提供一些小訣竅：

- 建議不要使用沒有時效性的 Channel Access Token ，建議使用 API 來要求。
- 使用 API 來申請 Channel Access Token 建議使用 v2.1 的方式來發出需求。 

如此一來除了可以確保整個頻道(channel) 憑證的安全性，必要時也可以將有暴露考量的 token 撤銷掉。

#### 參考文章:

- [LINE Dev Doc: Issue channel access tokens v2.1](https://developers.line.biz/en/docs/messaging-api/generate-json-web-token/)

## B. Channel Access Token 自動更新

<script async class="speakerdeck-embed" data-slide="23" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

接續前一頁，針對 Channel Access Token 的管理上。建議使用短期有效的 Channel Access Token ，並且在期限即將到期的時候， Issue 新的 Token。 請注意 Access Token 個數有上限（下一頁解釋），所以超過個數時需要將多的撤銷 (Revoke) 掉。

## C. Channel Access Token 有效上限數量

<script async class="speakerdeck-embed" data-slide="24" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這一篇有詳細敘述關於 Channel Access Token 的個數：

- Short-lived channel access token (短期) : 
  - **申請方式**： 透過 API ，參考[說明文件](https://developers.line.biz/en/docs/messaging-api/generate-json-web-token/#issue_a_channel_access_token_v2_1)。
  - **個數**：  30 個
  - **期限**:  30 天
- Long-lived channel access token (長期):
  - **申請方式**： LINE Developer Console 
  - **個數**: 1 個
  - **期限**： 直到重新申請為止。

#### 相關文件：

-  [Issue channel access token v2.1](https://developers.line.biz/en/reference/messaging-api/#issue-channel-access-token-v2-1)
-  [Issue short-lived channel access token](https://developers.line.biz/en/reference/messaging-api/#issue-shortlived-channel-access-token)

## D. 訊息發送完成後接收回應

<script async class="speakerdeck-embed" data-slide="25" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這一頁投影片是敘述，如果開發者使用  [Send push message](https://developers.line.biz/en/reference/messaging-api/#send-push-message) 或是  [Send reply message](https://developers.line.biz/en/reference/messaging-api/#send-reply-message) 的系統伺服器的回應狀況。通常成功的話，就不會回覆任何資訊（empty json ) ，如果有誤才會回覆詳細的錯誤資訊。

#### 相關文件：

-  [Send push message](https://developers.line.biz/en/reference/messaging-api/#send-push-message) 
-  [Send reply message](https://developers.line.biz/en/reference/messaging-api/#send-reply-message) 

## E. API 請求重試

<script async class="speakerdeck-embed" data-slide="26" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

有些時候發送大量的 API 呼叫的時候，因為有一些不可預期的狀況，造成 API 呼叫無法成功，或是無法收到回應的狀況。這時候為了能夠確認前一次的呼叫是否有成功，平台這邊有設計相關的重試 (Retry) 機制可以檢查。

![](https://developers.line.biz/assets/img/retry-key-flowchart-en.df00acef.png)

透過 “Safely retrying” 機制。 可以讓開發者測試一下上次的訊息是否有正確的發送成功，並且也可以確保有無任何的使用者被漏發了。 相關的使用情境如下：

- 上次不知道有無法送完成，呼叫 “Safely retrying” 可以重複發送同一則訊息。 有收過得不會收到重複訊息，沒收到的可以確保收到。
- 上次發送發生了平台無法完成指令的意外，透過 “Safely retrying” 可以跟平台確認上次的狀況。 如果上次有完整發送完畢，也不會有重複計費的疑慮。

#### 相關文件

- [Retrying a failed API request](https://developers.line.biz/en/docs/messaging-api/retrying-api-request/)



## F. 請求的相關限制

<script async class="speakerdeck-embed" data-slide="27" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

開發者在使用 API 的時候應該要避免大量的呼叫 API 超過設定的 Rate Limit 而造成系統判斷為惡意的呼叫。 其中 [Rate Limits](https://developers.line.biz/en/reference/messaging-api/#rate-limits) 可以參考以下相關訊息：

- 比較需要處理資源部分的 API 呼叫都是 一小時 60 次。
  - [Send a narrowcast message](https://developers.line.biz/en/reference/messaging-api/#send-narrowcast-message)
  - [Send a broadcast message](https://developers.line.biz/en/reference/messaging-api/#send-broadcast-message)
  - [Get number of sent messages](https://developers.line.biz/en/reference/messaging-api/#get-number-of-delivery-messages)
  - [Get number of friends](https://developers.line.biz/en/reference/messaging-api/#get-number-of-followers)
  - [Get friend demographics](https://developers.line.biz/en/reference/messaging-api/#get-demographic)
  - [Get user interaction statistics](https://developers.line.biz/en/reference/messaging-api/#get-message-event)
  - [Test webhook endpoint](https://developers.line.biz/en/reference/messaging-api/#test-webhook-endpoint)
- 處理資源較少的 API 可以接受一分鐘 60 次的呼叫。
  - [Create audience for uploading user IDs (by JSON)](https://developers.line.biz/en/reference/messaging-api/#create-upload-audience-group)
  - [Create audience for uploading user IDs (by file)](https://developers.line.biz/en/reference/messaging-api/#create-upload-audience-group-by-file)
  - [Add user IDs or Identifiers for Advertisers (IFAs) to an audience for uploading user IDs (by JSON)](https://developers.line.biz/en/reference/messaging-api/#update-upload-audience-group)
  - [Add user IDs or Identifiers for Advertisers (IFAs) to an audience for uploading user IDs (by file)](https://developers.line.biz/en/reference/messaging-api/#update-upload-audience-group-by-file)
  - [Create audience for click-based retargeting](https://developers.line.biz/en/reference/messaging-api/#create-click-audience-group)
  - [Create audience for impression-based retargeting](https://developers.line.biz/en/reference/messaging-api/#create-imp-audience-group)
  - [Rename an audience](https://developers.line.biz/en/reference/messaging-api/#set-description-audience-group)
  - [Delete audience](https://developers.line.biz/en/reference/messaging-api/#delete-audience-group)
  - [Get audience data](https://developers.line.biz/en/reference/messaging-api/#get-audience-group)
  - [Get data for multiple audiences](https://developers.line.biz/en/reference/messaging-api/#get-audience-groups)
  - [Get the authority level of the audience](https://developers.line.biz/en/reference/messaging-api/#get-authority-level)
  - [Change the authority level of the audience](https://developers.line.biz/en/reference/messaging-api/#change-authority-level)
- 有一些測試或是檢驗類型的則可以更高，到達一分鐘 1000 次。
  - [Set webhook endpoint URL](https://developers.line.biz/en/reference/messaging-api/#set-webhook-endpoint-url)
  - [Get webhook endpoint information](https://developers.line.biz/en/reference/messaging-api/#get-webhook-endpoint-information)

如果超過了這些次數的限制，則會獲得 `420 Too Many Requests` 的回應。請開發者們一定要注意。

#### 相關文章:

- [Rate limits](https://developers.line.biz/en/reference/messaging-api/#rate-limits)

- [Prohibiting mass requests to the LINE platform](https://developers.line.biz/en/docs/messaging-api/development-guidelines/#prohibiting-mass-requests-to-line-platform)

## G. 回應 ( reply ) 訊息與推播( push )訊息

<script async class="speakerdeck-embed" data-slide="28" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這邊則是詳細敘述關於 Reply Message 跟 Push Message 的差別：

- Reply Token 有時效性。更多資訊可以參考  「[開發LINE聊天機器人不可不知的十件事](https://engineering.linecorp.com/zh-hant/blog/line-device-10/)」  裡面的相關註解。
- LINE Messaging API的Webhook的下列事件物件會帶有Reply token：message、follow、join、postback與beacon。使用Reply token傳送訊息請注意以下二點：
  - Reply token的有效期間非常短，在收到Webhook事件後必須盡快使用。有效期間會隨著系統狀況而調整，所以我們也不便對外提供精確的數字。可以確定的是這個數字會以秒為單位，開發者是無法以Reply token回覆需要經過數分鐘以上處理時間才能獲得結果的訊息。這個目的是希望開發者能夠在最短的時間內回覆用戶的訊息，提供更好的使用者體驗。
  - Reply token僅可以使用一次，如果有需要在收到Webhook事件後分多次回覆，就必須使用[Push message](https://devdocs.line.me/en/#push-message)的方式來傳送訊息。

#### 相關文件：

-  [Send push message](https://developers.line.biz/en/reference/messaging-api/#send-push-message) 
-  [Send reply message](https://developers.line.biz/en/reference/messaging-api/#send-reply-message) 
-  [開發LINE聊天機器人不可不知的十件事](https://engineering.linecorp.com/zh-hant/blog/line-device-10/)

## H. HTTPS 內容的使用

<script async class="speakerdeck-embed" data-slide="29" data-id="0e9f6182ae864568a5940cbad5ef4bec" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

如果在訊息內需要加上圖片，影片，音訊等等相關資料。記得除了需要符合 HTTPS 的安全規範外，還需要符合 TLS 1.2 以上的安全規範，不然將無法正確顯示。 將不在支援 TLS 1.0 與 1.1 （參考 [Updated: TLS 1.0 and TLS 1.1 support by the webhook notification source will be discontinued at the end of January 2021](https://developers.line.biz/en/news/2020/10/06/update-webhook-client-a nd-root-certificate/))

#### 相關文件：

- [[Updated\] TLS 1.0 and TLS 1.1 support by the webhook notification source will be discontinued at the end of January 2021](https://developers.line.biz/en/news/2020/10/06/update-webhook-client-and-root-certificate/)

- 「[開發LINE聊天機器人不可不知的十件事](https://engineering.linecorp.com/zh-hant/blog/line-device-10/)」

## 結論：

<a id="summary"></a>

以上就是「LINE Bot 開發指南」第三部分的補充與分享，想要知道更多內容可以查看完整投影片，或是找到其他篇的文章來了解。 

想了解更多開發者的活動？  立即加入「LINE 開發者官方社群」官方帳號，就能收到第一手 Meetup 活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE 開發者官方社群」官方帳號 ID：@line_tw_dev
![img](https://www.evanlin.com/images/2020/line-tw-dev-qr.png)

