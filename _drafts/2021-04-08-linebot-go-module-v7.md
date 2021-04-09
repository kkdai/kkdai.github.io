---
layout: post
title: "[學習心得][Golang] 開源專案開啟 Go Modules 可能會遇到的問題"
description: ""
category: 
- 學習文件
tags: ["Golang", "DevOps", "CICD"]
---



![](/Users/evanlin/Downloads/Go_SDK.png)

## 前言:





## TL;DR 

本篇文將要介紹：

- <a href="#try-go">如何安裝嚐鮮版本的 golang 1.16</a>
- <a href="#116">1.16 新功能主要列表 </a>
  - <a href="#m1"> Apple Sillicon (也就是目前的 Apple M1 Chip) support </a>
  - <a href="#retract"> Go Module Retract (撤回發佈有問題的套件)  </a>
  - <a href="#embed"> Embedding Files  (把靜態檔案包含在專案中) </a>




## 如何安裝嚐鮮版本的 golang 1.16 

<a id="try-go"></a>

如果你想要嘗試一下，還沒有在 Homebrew 上有支援的 Golang 版本，就目前 (2021/02/19) 狀況由於許多相關套件還沒有更新好，造就 [HomeBrew 遲遲無法 Merge ，大家可以參考一下這個 PR](https://github.com/Homebrew/homebrew-core/pull/71289) 。

那如何在本地端安裝一下測試版本的 Go1.16 呢？ 就如同本文開場圖片的敘述一下：

```
go get golang.org/dl/go1.16
go1.16 download
```

如此一來，就會在本地端安裝一個編譯好的檔案。 `go1.16` 如果需要相關的測試可以直接跑 `go1.16 build` 或是 `go1.16 test` 來跑。



## 1.16 新功能主要列表

<a id="116"></a>

### Apple Sillicon (也就是目前的 Apple M1 Chip) support

<a id="m1"></a>

這個版本正式支援 apple Silicon 誒就是 64-bit ARM 架構。（a.k.a. Apple M1 chip) 。 可以透過 compiler 參數:

- `GOOS=darwin`, 
- `GOARCH=arm64`

來設定，而原先的 iPhone binary 設定則改為：

- `GOOS=darwin`, 
- `GOARCH=ios/arm64`

可以透過指令 `env GOOS=darwin GOARCH=arm64 go build` 來編譯出給 Apple M1 的 binary 。



### Go Module Retract
<a id="retract"></a>

這部分可以參考我的另外一篇詳細文章。 [[學習心得\][Golang] Go 1.16 新功能的「版本撤回(下架)」(Go Modules retraction)](http://www.evanlin.com/til-go-retract/)

### Embedding Files (把靜態檔案包含在專案中)
<a id="embed"></a>

以往是無法將靜態檔案，包在 Golang 的專案之中。幾個方式只有：

- 如果要載入的檔案是 json ，將它弄成變數。
- 如果是 html 的 template 檔案，需要跟 binary 放在一起
- 或是可以看一下 [go-bindata](https://github.com/go-bindata/go-bindata) 的專案(相似的還有 [packr](https://github.com/gobuffalo/packr) 跟 [pkger](https://github.com/markbates/pkger) )，透過這個方式將 static file 放在專案中變成 resource 。

但是在 1.16 之後，可以正式支援了。

假設檔案結構為:

```
.
├── go.mod
├── main.go
├── static
│ └── css
│ └── main.css
├── templates
│ └── index.html.tmpl
└── title.txt

```

透過以下方式，可以將檔案打包到專案中：

<script src="https://gist.github.com/kkdai/111803ace159cdaf87e7d855d701689b.js"></script>

以後要打包整個網站，不用在擔心 docker 打包的時候會忘記把 template 跟 image 資源檔案忘記打包。

#### 相關資料

- [Go Doc: Embed Files](https://golang.org/doc/go1.16#embed)

- [Embedding static files in a go binary using go embed](https://harsimranmaan.medium.com/embedding-static-files-in-a-go-binary-using-go-embed-bac505f3cb9a)

- [How to embed files into Go binaries](https://stackoverflow.com/questions/17796043/how-to-embed-files-into-go-binaries)




## 相關文章：

- [Go 1.16 is released](https://blog.golang.org/go1.16)

- [Retract Go Module Versions in Go 1.16](https://golangtutorial.dev/tips/retract-go-module-versions/)

- [Go 1.16 中原本欲支持的结构体字段标签合并写法特性被取消了](https://mp.weixin.qq.com/s/7eLLhHt8hsTd6hmzj1AWTw)
