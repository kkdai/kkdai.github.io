---
layout: post
title: "[學習心得][Golang] 如何透過 Golang 開發 OAuth2 的 PKCE - 以 LINE Login SDK 為例"
description: ""
category: 
- 學習文件
tags: ["Golang", "OAuth2", "PKCE"]
---

<img src="../images/2021/Go_SDK.png" width="400px">


##前言:

[在 2021/04/09 的新聞](https://developers.line.biz/en/news/2021/04/09/line-login-pkce-support/)上， LINE Login 正是支持了 PKCE (Proof Key for Code Exchange) 的流程。 本篇文章將清楚地解釋一下，什麼是 LINE Login ？ 為何 LINE Login 需要支持 PKCE ? 最後會透過一個範例，帶領著讀者們一起來支持 LINE Login with PKCE 。



# TL;DR 

本篇文將要介紹以下一些的部分：

- <a href="#line-login-oauth">什麼是 LINE Login? 什麼又是 OAuth ? </a>
- <a href="#when-line-login">什麼樣的情況會建議使用 LINE Login?</a>
- - <a href="#oauth-issue">OAuth2 有什麼樣的缺點?</a>
- <a href="#what-is-pkce">什麼是 PKCE?</a>
- <a href="#how-to-migrate-pkce">如何在 LINE Login 之中導入 PKCE?</a>
- <a href="#summary">結論</a>
- <a href="#refer">參考文章</a>
- 


# 什麼是 LINE Login? 什麼又是 OAuth 2.0 ? 

<a id="line-login-oauth"></a>

![](https://developers.line.biz/media/services/line-login-main-contents.png)



許多的商業服務都會透過會員機制來提供許多專屬的優惠或是獎勵活動，但是會員的註冊與登入流程常常讓許多使用者覺得為難。除了要填寫許多的資料外，使用者還需要額外記住另外一組的帳號密碼。 LINE 在台灣的佔有率相當的高，並且幾乎每個使用者都有 LINE 的帳戶的狀況下，這時候如果能夠直接使用 LINE 帳戶來註冊與登入網站服務的話是不是相當的方便？

LINE Login 除了提供一個方式來登入之外，也可以提供使用者名稱，大頭照的相關資訊。並且透過 LINE Login 也可以同時讓使用者加入商業服務的 LINE官方帳號，讓使用者更無時無可都可以使用到相關的服務。



# 什麼樣的情況會建議使用 LINE Login
<a id="when-line-login"></a>

這裡會條列出哪些情況建議需使用 LINE Login 作為讀者來評量自己有沒有需要使用 LINE Login :

- 剛開始要建立電子商務服務或是網站，想要減少使用者註冊的時間並且快速加入。
- 想要推廣自身官方帳號的聊天機器人服務。
- 就算是已經推廣一段時間的電子商務服務，但是想要透過 LINE 來接觸不同的客戶群。

了解為什麼使用 LINE Login 以及甚麼狀況下建議使用之後，接下來就引導讀者如何使用範例程式碼



## OAuth 有什麼樣的缺點

<a id="oauth-issue"></a>
![](https://developers.line.biz/assets/img/new-user-login-without-pkce-en.54bd0a4b.svg)



![](../images/2021/pkce-diagram.jpg)



##   什麼是 PKCE? 

<a id="what-is-pkce"></a>

![](https://developers.line.biz/assets/img/new-user-login-with-pkce-en.8be182f5.svg)

## 如何在 LINE Login 之中導入 PKCE?
<a id="how-to-migrate-pkce"></a>



## 結論：

<a id="summary"></a>





## 相關文章：
<a id="refer"></a>

-  [LINE Login now supports PKCE](https://developers.line.biz/en/news/2021/04/09/line-login-pkce-support/)

- [RFC7636 -  Proof Key for Code Exchange by OAuth Public Clients](https://tools.ietf.org/html/rfc7636)

- [RFC7523 - JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7523)

- [RFC6749- The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)

-  [Developer News: LINE Login now supports PKCE](https://developers.line.biz/en/news/2021/04/09/line-login-pkce-support/)

- [Developer Doc - PKCE support for LINE Login](https://developers.line.biz/en/docs/line-login/integrate-pkce/#how-to-integrate-pkce)

- [OAuth 2.0 in Go](https://levelup.gitconnected.com/oauth-2-0-in-go-846b257d32b4)