---
layout: post
title: "[TIL][Golang] go/build 與 golang.org/x/tools/go/packages 的差異"
description: ""
category: 
- TodayILearn
- golang
tags: ["Golang", "Go"]
---

![](https://golang.org/lib/godoc/images/go-logo-blue.svg)

## 前言：

一開始主要是因為看到這個 tweet ，不禁想了解到底什麼是 `go/build` 而什麼又是 `golang.org/x/tools/go/packages` 。

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">TIL: Use <a href="https://t.co/eImdr3M4th">https://t.co/eImdr3M4th</a> instead of go/build. It is support modules, easier to use and faster. <a href="https://twitter.com/hashtag/gophercon?src=hash&amp;ref_src=twsrc%5Etfw">#gophercon</a> <a href="https://t.co/EruKtWest0">pic.twitter.com/EruKtWest0</a></p>&mdash; Jaana B. Dogan (@rakyll) <a href="https://twitter.com/rakyll/status/1154431863689076736?ref_src=twsrc%5Etfw">July 25, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
因為 "Developer tools should now use that instead. It support both go path and Go Mod." 這句話從 @_rsc 講出來更讓筆者想了解這些工具的差異。





## What is go/build? 

根據 [go/build 的 GoDoc](https://golang.org/pkg/go/build/) 上面解釋，其實 `go/build` 主要是幫助你取得 Go packages 資訊之用的，當然本身也可以直接拿取原本 binary 的 `GOARCH`, `GOROOT` 跟 `GOPATH` 的相關資訊。 當然也是可以取得 packages 相關資訊，至於使用上可以參考以下 sample code. 

### Sample code for go/build

<script src="https://gist.github.com/kkdai/2c424fcc4320bd77bd276c99c55dfc5b.js"></script>
這個片段程式碼可以解析使用者輸入的套件參數，並且應出所有的套件資訊。比如說你輸入 `gobuild flag` 就會解析 `flag`這個套件的相關資訊。



## What is golang.org/x/tools/go/packages?

而`golang.org/x/tools/go/packages` 比較專注做套件的解析工作，可以透過 [GoDoc](https://godoc.org/golang.org/x/tools/go/packages) 相關資訊來了解，主要透過 `packages.Load()` 來讀取所有的套件資訊，並且可以透過 `packages.Visit()`來將所有套件的 dependency 找出來。

而當初設計這個套件主要就是為了 Go Modules 而設計的，大家可以參考相關文件  [ The Go Blog: Go Modules in 2019](https://blog.golang.org/modules2019) ，裡面的相關敘述。 在 [Go 1.11 的 Release note](https://tip.golang.org/doc/go1.11#gopackages) 其實也有提到，在作為 Package Loading 的工具查詢上，雖然 `golang.org/x/tools/go/packages` 還沒納入 standard library ，但是強烈建議要做 package loading 相關事項的時候應該就要改成 packages 。

### Sample code for golang.org/x/tools/go/packages

<script src="https://gist.github.com/kkdai/0c0a54d4b9709c4c3afd740e07fe56eb.js"></script>
## 小結:

`golang.org/x/tools/go/packages` 在查詢套件資訊的時候，會先透過 GoPath 來查詢，如果找不到就會透過 Go Modules 來查詢。 而 `go/build` 只要 GoPath 找不到就會顯示套件無法查詢。

這兩個工具在做一些 DevOps 工具或是一些 CICD 的相關工作的時候會用到，就如同 @_rsc 建議的，應該要儘早使用 `golang.org/x/tools/go/packages` 來查詢相關資訊。但是可惜的是  [Bazel](https://bazel.build/) 目前還沒有支援。

## **Reference:**

- https://twitter.com/rakyll/status/1154431863689076736
- https://godoc.org/golang.org/x/tools/go/packages
- https://golang.org/pkg/go/build/
- https://github.com/kkdai/build-package-test
- https://blog.golang.org/modules2019
- https://tip.golang.org/doc/go1.11#gopackages



