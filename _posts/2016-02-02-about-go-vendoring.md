---
layout: post
layout: post
title: "[Golang] 關於Go的Vendoring"
description: ""
category: 
- golang
tags: [go]
---

![](https://blog.golang.org/6years-gopher.png)

##  起因

本來對於Golang的vendoring都是統一使用[godep](https://github.com/tools/godep)(如果比較大的專案)，比較小的專案都直接使用`go get`來抓取dependency package． 

但是由於[Google Code](https://code.google.com/)在2016/01/25關閉了，所以常用的uuid沒得抓了．所以來試試看Go 1.5 開始的[vendor experiment](https://docs.google.com/document/d/1Bz5-UB7g2uPBdOx-rw5t9MxJwkfpx90cqG9AFL0JAYo/edit)，順便做點記錄．

## 什麼是vendor experiment?

一般而言我們在使用Golang套件的時候都是很簡單的直接使用

```go
import "github.com/kkdai/coapmq"
```

這樣只要編譯專案前先跑 `go get -d`就會把所有的相依專案都抓取**最新**的程式碼到你的`GOPATH`． 現在問題來了:

- 引用的專案已經被砍掉了？
- 引用的專案原作者或是維護團隊忽然決定大改API?
- 引用的專案忽然出現意外的錯誤(break code)? (當然有可能是底層API被Google改掉)
- 想要維護整個專案的穩定度

根據以上的需求或是要避免以上的一些問題．將你的專案Vendoring是必要的．而Vendoring的做法一般就是放一份你目前使用的相依套件的程式碼到 `vendor`目錄底下． 有可能是這樣

```
- vendor
  - github.com
	  - kkdai
		  - coapmq
		  - photomgr 

```

## 開始將你的專案轉換成可以使用vendor experiment

假設你現在有一個專案裡面用到了數個相依的套件(ex: [mysql](https://github.com/go-sql-driver/mysql)，[uuid](code.google.com/p/go-uuid/uuid) (注意此鏈結已經失效)
... ）

建議流程如下:

#### 確認你的專案是在GOPATH底下

不論是GO15VENDOREXPERIMENT還是govendor都是使用在GOPATH才能作用．所以請注意:

- **專案一定要放GOPATH底下**
- **專案一定要放GOPATH底下**
- **專案一定要放GOPATH底下**

如果不在GOPATH底下，將不會work．我想這也是[godep](https://github.com/tools/godep)或是[GB](https://getgb.io)能夠繼續讓大家熱愛的原因．


#### 設定GO15VENDOREXPERIMENT=1

一開始得把這個flag打開，你可以先使用`go env`來查詢該flag是不是有開啟的狀態（初始是關閉，但是Go 1.6將會打開)

- Mac/Ubutu: `export GO15VENDOREXPERIMENT=1`
- Windows: `set GO15VENDOREXPERIMENT=1`

#### 將你相依的專案列出來

這裡使用的是[govendor](https://github.com/kardianos/govendor)，透過他可以將所有相依的專案．流程如下:

- `govendor init`來初始化，並且將所有的專案寫入vendor.json裡面．
- 這時候你可以透過`govendor list`來列出所有的相依專案，然後決定哪個要放入vendor資料夾
- 假設你要把`github.com/kkdai/coapmq`放入vendor目錄中，就執行以下 `govendor add github.com/kkdai/coapmq`

依序把你需要的專案放在目錄中，然後就可以了．

#### 如何透過Vendoring Experiment編譯檔案 

將你需要的專案抓到`GOPATH`底下（**必須**)，然後切記`GO15VENDOREXPERIMENT=1`，其實就簡單地跑`go get`然後`go build`就可以了．


### 其他Vendoring的用法

其實還有其他方式，不論是[Godep](https://github.com/tools/godep)或是[GB](https://getgb.io) 其實都各有愛護者，不過用的習慣與相關的使用場景是比較重要的．

## 相關鏈結

- [Go 1.5 Vendor Experiment - Google Docs](https://docs.google.com/document/d/1Bz5-UB7g2uPBdOx-rw5t9MxJwkfpx90cqG9AFL0JAYo/edit)
- [Govendor](https://github.com/kardianos/govendor)
- [GB: A project based build tool for the Go programming language.](https://getgb.io) 
- [Godep](https://github.com/tools/godep)
- [William Kennedy - Dependency Management](https://www.youtube.com/watch?v=CdhucJShJU8&list=PLDWZ5uzn69ezRJYeWxYNRMYebvf8DerHd)
- [Go Wiki: PackageManagementTools](https://github.com/golang/go/wiki/PackageManagementTools)
- [理解Go 1.5 vendor](http://tonybai.com/2015/07/31/understand-go15-vendor/)
