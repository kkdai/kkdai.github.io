---
layout: post
title: "[Golang][LINE][教學] 導入 LINE Login 到你的商業網站之中，並且加入官方帳號為好友"
description: ""
category: 
- LINE
- 學習心得
- Golang
tags: ["LINE", "golang", "LINE login"]
---

![](https://developers.line.biz/media/line-login/link-a-bot/consent-screen-with-bot-54e042bf.png)

(圖片來自: [Linking a bot with your LINE Login channel](https://developers.line.biz/en/docs/line-login/web/link-a-bot/) )



# 前言

剛起步的商業網站或是服務，想要吸引使用者來註冊，但是由於需要輸入一堆資訊讓使用者望之卻步。這個時候透過 OpenID 的登入方式常常可以吸引許多使用者快速加入服務。不論是透過透過 Google 或是  Facebook 的帳號都是很方便的。做為台灣手機軟體佔有率名列前茅的 LINE ，當然也有相關的服務稱為 LINE Login 。

繼上一篇 [[Golang][LINE][教學] 將你的 chatbot 透過 account link 連接你的服務](http://www.evanlin.com/line-accountlink/) 教學之後，本篇文章將介紹 **LINE Login** 並且提供一個範例 (Golang)  講解如何導入 LINE Login 在已經有的商業網站服務之中。 

常常也很多開發者在詢問，如何透過 LINE Login 在使用這加入服務的時候直接幫使用者加入官方帳號。這篇文章也會在最後的範例裡面提到該如何做。



# 為何要使用 LINE Login 

許多的商業服務都會透過會員機制來提供許多專屬的優惠或是獎勵活動，但是會員的註冊與登入流程常常讓許多使用者覺得為難。除了要填寫許多的資料外，使用者還需要額外記住另外一組的帳號密碼。  LINE 在台灣的佔有率相當的高，並且幾乎每個使用者都有 LINE 的帳戶的狀況下，這時候如果能夠直接使用 LINE 帳戶來註冊與登入網站服務的話是不是相當的方便？

LINE Login 除了提供一個方式來登入之外，也可以提供使用者名稱，大頭照的相關資訊。並且透過 LINE Login 也可以同時讓使用者加入商業服務的 LINE官方帳號，讓使用者更無時無可都可以使用到相關的服務。



# 什麼樣的情況會建議使用 LINE Login

這裡會條列出哪些情況建議需使用 LINE Login 作為讀者來評量自己有沒有需要使用 LINE Login :

- 剛開始要建立電子商務服務或是網站，想要減少使用者註冊的時間並且快速加入。
- 想要推廣自身官方帳號的聊天機器人服務。
- 就算是已經推廣一段時間的電子商務服務，但是想要透過 LINE 來接觸不同的客戶群。



了解為什麼使用 LINE Login  以及甚麼狀況下建議使用之後，接下來就引導讀者如何使用範例程式碼



# 範例程式碼

https://github.com/kkdai/line-login-go



# 如何部署範例程式碼:

- 到 [LINE Developer Console](https://developers.line.biz/console/) 建立相關的 Provider 跟 Channel。

- 建立一個 LINE Login 的帳號，並且將以下兩個資訊記住:

  - Channel ID
  - Channel Secret

- 另外建立一個 [LINE@](https://at.line.me/tw/) 並且打開 MessageAPI 的功能（也就是建置 chatbot 用），並且將以下兩個資訊記住:

  - Channel Secret
  - Channel Token

- 到 https://github.com/kkdai/line-login-go 按下  Heroku Deploy ，建立該帳號並且部署該服務。這時候會要輸入三個資訊:

  - LINECORP_PLATFORM_CHANNEL_CHANNELID
    - 填入 LINE login channel ID
  - LINECORP_PLATFORM_CHANNEL_CHANNELSECRET
    - 填入 LINE login channel Secret
  - LINECORP_PLATFORM_CHATBOT_CHANNELSECRET
    - 填入 Chatbot  channel Secret
  - LINECORP_PLATFORM_CHATBOT_CHANNELTOKEN
    - 填入 Chatbot  channel Token

  - LINECORP_PLATFORM_SERVERURL
    - 這個資訊根據你的 heroku app 名稱來決定，假設你的 Heroku app 名稱叫做 `test-api-1234` 那麼你就該填 `https://test-api-1234.herokuapp.com/`

- 回到 LINE Login 的帳號設定，[App setting] 將以下位置寫入 callback URL `https://test-api-1234.herokuapp.com/auth`

- 回到 LINE chatbot 的帳號設定，記得把 `https://test-api-1234.herokuapp.com/callback` 加到 LINE chatbot web hook 才能正確地啟動聊天機器人。

# 實際跑一個範例

![](../images/2019/linelogin-1.PNG)

- 當部屬好網站服務後，直接用瀏覽器登入網站即可。如果沒有部署的人，可以使用 https://login-tester-evan.herokuapp.com/
- 連線到網站後會看到有三個 LOGIN botton 選項
  - LINE Web Login
  - LINE Web Login with chatbot (normal)
  - LINE Web Login with chatbot (aggressive)
- 如果你選擇第一個登入，就會直接跳到 LINE Login 的選項。
- 如果你選擇第二個選項，在 Consent Page (使用者同意頁面) 會多一個是否加入官方帳號的選項。
- 第三個跟第二個類似．只是會將加入官方帳號的選項另外跳出一夜。讓使用者更容易加入。



# 流程圖與程式碼解釋



![](https://developers.line.biz/media/line-login/integrate-login-web/login-flow-web-0bc4c99d.png)

(圖片來自: [Integrating LINE Login with your web app](https://developers.line.biz/en/docs/line-login/web/integrate-line-login/))

以下的內容均參照 [Integrating LINE Login with your web app](https://developers.line.biz/en/docs/line-login/web/integrate-line-login/) 

- (1). 首先瀏覽器訪問該網站 （假設你直接使用  `https://login-tester-evan.herokuapp.com/`)，進入了 `browse()` 顯示出三個 LINE Login 按鈕。
- (2). 使用者按下了 LINE Web Login 的話，就會進入 `gotoauthpage()` 直接產生一組 API URL 直接導向到 `https://access.line.me/oauth2/v2.1/authorize` 。這邊其實有一些參數可以設定，可以參考 [LINE Login 參數設定](https://developers.line.biz/en/docs/line-login/web/integrate-line-login/#spy-making-an-authorization-request)。
- (3). 使用者這時候會連到 LINE Platform 來進行登入的步驟，不論是透過 App 登入還是輸入帳號密碼的登入方式。登入完成後會透過 `redirect_uri` 網址的位址傳回一組 code ，作為使用者資料的存取之用。 這時候會呼叫到 `auth()` 來解析 code 。
- (4). 在同一個 `auth()` 之中解析 code, state, 與 friendship_status_changed 後，檢查 state 正確性之後。就可以透過 code 去抓取使用者的資料了。 這時候會呼叫 `https://api.line.me/oauth2/v2.1/token` 的 API ，相關必要參數也可以參考[這個網址](https://developers.line.biz/en/docs/line-login/web/integrate-line-login/#spy-getting-an-access-token)。
- (5). 一樣在 `auth()` 之中，回傳的結果可以得到 id_token ，透過 id_token 需要透過解譯的方式來將它還原成使用者的資料。這邊也已經包裝好為 `DecodeIDToken()`
- 透過 `DecodeIDToken()` 就可以得到使用這名稱與大頭照。 (email 需要額外申請)

以上就是一般的電子商務網站的導入流程，接下來就介紹如何在導入 LINE Login 的時候直接加入官方帳號 (聊天機器人) 為好友的步驟。


# 在 LINE Login 同時直接加入官方帳號為好友

根據 [Linking a bot with your LINE Login channel](https://developers.line.biz/en/docs/line-login/web/link-a-bot/)的教學，以下將流程簡化讓大家能夠快速了解。讀者可以直接透過範例網站或是程式碼來感受之間的差異性。主要就是 `bot_prompt={BOT_PROMPT}` 這個參數的差別。

- 使用者點下 LINE Web Login with chatbot (normal) 的按鈕，則會送出 `bot_prompt=normal` 。在使用者同意頁面會出現官方帳號的選項給使用者選取。
- 使用者點下 LINE Web Login with chatbot (aggressive) 的按鈕，則會送出 `bot_prompt=aggressive`。這時候會跳出一個新的頁面讓使用者來使用者選取是否要加入官方帳號。

這邊提出幾個建議在接受 `friendship_status_changed` 的時候，需要注意的事項:

- 當 `friendship_status_changed` 回傳為 true 的時候，這時候需要透過 `https://api.line.me/friendship/v1/status` 的 API 再去確認好友狀態的變化。
- 如果有加入好友的話，建議也可以緊接著進行 account-link 的相關設定。在 web login session 還維持的時候可以快速將使用者的 chatbot 與帳號鏈結在一起。



# 總結

對於一個新的服務，如何快速的讓使用者能夠註冊與登入是一個大難題。這時候透過 LINE Login 可以幫你解決大多數的問題，並且 LINE Login 在手機上往往可以直接開啟 LINE App 使用者連登入的流程都省略了。馬上就註冊完畢與登入。  使用者也不需要擔心， LINE Login 僅僅提供名字與大頭照作為基本資料( Email 需要申請 ) 。

本篇文章透過一個簡單的 Golang 範例，展示了如何快速導入 LINE  Login 到電子商務網站之外。還指引了如何透過 LINE Login 來加入官方帳號的聊天機器人到使用者的 LINE 帳戶。 讓使用者在透過 LINE Login 的同時也馬上了解該服務的 LINE 官方帳號的位置。並且可以提供更多元的服務。


# 參考

- [Linking a bot with your LINE Login channel](https://developers.line.biz/en/docs/line-login/web/link-a-bot/)
-  [Integrating LINE Login with your web app](https://developers.line.biz/en/docs/line-login/web/integrate-line-login/) 
