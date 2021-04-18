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








## TL;DR 

本篇文將要介紹以下一些的部分：

- <a href="#legacy-support-go-modules">如何將舊的開源專案支援 Go Modules </a>
- <a href="#summary">結論</a>
- <a href="#refer">參考文章</a>
  


## 如何將舊的開源專案支援 Go Modules 

<a id="legacy-support-go-modules"></a>

<img src="../images/2021/Go_SDK.png" width="400px">

LINE-BOT-SDK-GO 是 LINE 開源出來的對於 LINE Messanging API 所釋放出的開源套件，並且支援多個語言版本（Go., PHP, Java, Python) 。 

原本這個 https://github.com/line/line-bot-sdk-go 的版本已經超過 v7 ，但是遲遲沒有支援 go modules 。 也就是並沒有 `go.mod` 在該專案的檔案下面。所以需要透過以下方式來啟動 Go Modules (Enable Go Modules)

```
- go mod init
- go mod tidy
- go mod vendor
```




## 結論：

<a id="summary"></a>





## 相關文章：
<a id="refer"></a>

- [RFC7636 -  Proof Key for Code Exchange by OAuth Public Clients](https://tools.ietf.org/html/rfc7636)

- [RFC7523 - JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://tools.ietf.org/html/rfc7523)

- [RFC6749- The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)

-  [Developer News: LINE Login now supports PKCE](https://developers.line.biz/en/news/2021/04/09/line-login-pkce-support/)

- [Developer Doc - PKCE support for LINE Login](https://developers.line.biz/en/docs/line-login/integrate-pkce/#how-to-integrate-pkce)

- [OAuth 2.0 in Go](https://levelup.gitconnected.com/oauth-2-0-in-go-846b257d32b4)
