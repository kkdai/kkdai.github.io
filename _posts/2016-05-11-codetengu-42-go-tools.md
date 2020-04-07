---
layout: post
title: "[碼天狗#42] 讓你 Go 程式碼變得更好的工具集 (Tools to Help you Go)"
description: ""
category: 
- 碼天狗專欄
tags: ["go", "codetengu"]

---

![](https://cdn-images-1.medium.com/max/2000/1*RzE9hBgmsl6gaxCfO4n5Wg.jpeg)

本文刊登於 [碼天狗#42](http://weekly.codetengu.com/issues/42#start) 有興趣追蹤相關文章讀者，最快的方式就是訂閱[碼天狗](http://weekly.codetengu.com) ．

## 文章鏈結

### [https://serifandsemaphore.io/tools-to-help-you-go-d6f782055ce7#.1iex1ex8l](https://serifandsemaphore.io/tools-to-help-you-go-d6f782055ce7#.1iex1ex8l)



Golang 讓人喜歡的許多原因之一，就是有著許多好用的工具可以讓你在撰寫 Go 的時候更加的輕鬆而方便． 舉個最間單的例子，就是 coding style tool [goimport](https://github.com/bradfitz/goimports)  他可以讓你在存在之後，自動把 coding style 改成 Golang community 所習慣的範例． 當然還有好用到不行的 [guru](https://godoc.org/golang.org/x/tools/cmd/guru) 可以幫助你查詢 caller 與 callee．除了這兩個之外，這一篇有介紹更多的工具:

- [Golint](https://github.com/golang/lint) : 可以自動幫你檢查所有程式碼中潛在的語法問題．
- [Gocyclo](https://github.com/fzipp/gocyclo) : 可以幫你計算你程式碼中的循環複雜度（Cyclomatic complexity）避免有過多的迴圈運算．
- [Depscheck](https://github.com/divan/depscheck) :  可以幫你產生相依性的報表，讓你可以思考有多少小套套件是相依其他人的．經過了 [NPM 的 LeftPad ](http://www.thenewslens.com/post/305094/) 事件之後．其實不少人建議如果相依的套件太小，不仿自己寫過來吧．
- [errcheck](https://github.com/kisielk/errcheck) : 這個套件可以幫你檢查你的程式碼有沒有對於每個錯誤都有相對應的檢查與處理．避免明明有 error 卻沒有檢查的潛在問題．
- [safesql](https://github.com/stripe/safesql) : SQL Injection 是每個人在撰寫後端與資料庫橋接程式的時候最擔心的問題．這個工具可以幫你檢查看看有沒有危險的語法存在．

雖然 Golang 社群提供了許多的工具來幫助你，其實 Golang 本身的 Compiler 已經相當的強勢．會幫你檢查出許多潛在的問題．除了這些工具之外，當然相信自己與撰寫單元測試的好習慣都是可以幫助讓你的程式變得更好的不二法門．
