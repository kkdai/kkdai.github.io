---
layout: post
title: "[學習心得][Golang] 如何透過 Golang 開發 OAuth2 的 PKCE - 以 LINE Login SDK 為例"
description: ""
category: 
- 學習文件
tags: ["Golang", "OAuth2", "PKCE"]
---

<img src="../images/2021/Go_SDK.png" width="400px">


## 前言:

[在 2021/04/09 的新聞](https://developers.line.biz/en/news/2021/04/09/line-login-pkce-support/)上， LINE Login 正是支持了 PKCE (Proof Key for Code Exchange) 的流程。 本篇文章將清楚地解釋一下，什麼是 LINE Login ？ 為何 LINE Login 需要支持 PKCE ? 最後會透過一個範例，帶領著讀者們一起來支持 LINE Login with PKCE 。



## TL;DR 

本篇文將要介紹以下一些的部分：

- <a href="#line-login-oauth">什麼是 LINE Login? 什麼又是 OAuth ? </a>
- <a href="#oauth-issue">OAuth 有什麼樣的缺點?</a>
- <a href="#what-is-pkce">什麼是 PKCE?</a>
- <a href="#how-to-migrate-pkce">如何在 LINE Login 之中導入 PKCE?</a>
- <a href="#summary">結論</a>
- <a href="#refer">參考文章</a>
- 


## 什麼是 LINE Login? 什麼又是 OAuth 2.0 ? 

<a id="line-login-oauth"></a>

![](https://developers.line.biz/media/services/line-login-main-contents.png)

LINE Login 是一可以幫助開發者快速開發一個第三方登入的工具，透過 LINE App 的 Login ，使用者將不需要另外開發使用者資料的輸入方式，也更方便讓使用者不需要在另外登記一個密碼。



## OAuth 有什麼樣的缺點

<a id="oauth-issue"></a>



##   什麼是 PKCE? 

<a id="what-is-pkce"></a>

![](../images/2021/pkce-diagram.jpg)

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