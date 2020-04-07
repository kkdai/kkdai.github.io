---
layout: post
title: "[TIL][Golang] 使用 jQuery 來解析需要 cookies 的網頁內容"
description: ""
category: 
- TodayILearn
- Golang
tags: ["Golang", "Go", "jQuery"]
---

![](https://golang.org/lib/godoc/images/go-logo-blue.svg)

# 前言：

![](https://github.com/kkdai/iloveptt/blob/master/snapshot/1.png?raw=true)

就在農曆的除夕夜因為家裡的電視壞了好幾天，所以開始看自己的 Github 專案。想說來弄弄 Go modules ，結果發現這個專案已經不能用了。

 [iloveptt](https://github.com/kkdai/iloveptt) 是我有一個專門在爬 PTT 版面的小專案 ，最近拿起來用的時候忽然發現不能用了。 經過追查之下發現原本透過 GoJQuery 的功能套件忽然無法找尋到正確到資訊。 這一篇文章就是記錄著該如何尋找，還有該如何解決問題。

## 相關專案:

- 我愛批踢踢 A PTT Crawler and Photo downloader which written in Golang:  https://github.com/kkdai/iloveptt
- Golang: A photo download package for gomobile in Golang: https://github.com/kkdai/photomgr

## 問題發生了

原本要解析目錄的 jQuery 卻忽然無法成功，但是很確定找尋資料是正確的，也另外透過瀏覽器來查詢過網頁原始碼，沒有發現有相關的變更。

<script src="https://gist.github.com/kkdai/02e2af4f586208ef1ee678e585a817cd.js"></script>
這個時候首先需要把資料全部印出來，透過的方式可以先將所有資料的˙html 列出來，看看 jQuery 所得到的資料有什麼不同。  可以透過  `doc.Find("*").Each(func(i int, s *goquery.Selection) {` 的方式，並且使用 `s.Html()`來列印出目前搜尋到的真正結果問題在哪裡。



### 原來是網頁有使用者同意條款



這時候會發現這邊出現的 HTML 原始碼部分跟你在瀏覽器看到的不同，原來是 PTT 有顯示使用者同意條款，必須要使用者同意相關內容為 18歲以上的內容。 

而能夠正常讀取網頁的內容是因為你的瀏覽狀況有 cookies 。

而檢查  cookie 的方式可以透過 Chome Developer Console 透過 Networking 來尋找 Request Cookies 來查看是否有 Cookies 。 

參考相關文章： https://bryannotes.blogspot.com/2015/07/python-crawler.html 



## 把 JQuery 加上 Cookie 來查詢

那要回過頭來思考透過  github.com/PuerkitoBio/goquery 套件能不能夠加入 cookies ? 首先來查看一下文件好了。 https://godoc.org/github.com/PuerkitoBio/goquery#NewDocument  你會發現他有以下的方式。

- #### func [NewDocument](https://github.com/PuerkitoBio/goquery/blob/master/type.go#L38) 

- #### func [NewDocumentFromNode](https://github.com/PuerkitoBio/goquery/blob/master/type.go#L27) 

- #### func [NewDocumentFromResponse](https://github.com/PuerkitoBio/goquery/blob/master/type.go#L65) 

其中 [NewDocumentFromResponse](https://github.com/PuerkitoBio/goquery/blob/master/type.go#L65)  可以使用，我們就要開始思考如何透過 `net/http` 來取得具有 cookies 內容的資訊呢？

https://siongui.github.io/2018/03/03/go-http-request-with-cookie/ 這邊文章給了一個不錯的範例。

<script src="https://gist.github.com/kkdai/abc4944f17d87fd68dda07388005c07a.js"></script>
## 最後修改方式

參考以上的修改方式，我們需要將程式碼修改如下，才能夠正常的運行。

<script src="https://gist.github.com/kkdai/77d2061d0221d5e9996d412c1a5e5b7b.js"></script>
最後附上相關 issue number: https://github.com/kkdai/photomgr/issues/6



# 結論：

jQuery 很方便的可以直接透過 selector 來操作一些網頁上的資料，但是有時候透過 app browser  去抓取的資訊可能跟自己開瀏覽器的不同。 在除錯的流程上可能需要更加的小心才不會讓自己陷入找不到真正問題的所在而盲目猜測。

本篇文章希望能讓想透過 jQuery  寫 Golang 爬蟲的人一些點子，也可以幫助大家了解相關知識。

## **Reference:**

- Related issue https://github.com/kkdai/photomgr/issues/6
- https://bryannotes.blogspot.com/2015/07/python-crawler.html
- https://dometi.com.tw/blog/jquery-teach/
- https://siongui.github.io/2018/03/03/go-http-request-with-cookie/