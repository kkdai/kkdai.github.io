---
layout: post
layout: post
title: "[Golang] 利用build tags達到不同的build config"
description: ""
category: 
- golang
tags: []

---

##前言

![image](http://synflood.at/tmp/golang-slides/images/gophercolor.png)

為了要寫一個Web Service但是有可能會發生以下的需求:

- 不同Server，有不同的設定參數
- 不同的OS有不同的command set
- 想要快速的開啟或是關閉debug log

在有以上的需求的時候，一開始都是使用OS偵測或是configuration file來區隔．但是到後其實是希望能透過不同的build config能夠產生不同的binary．

決定研究了一下: go build 有以下兩種方式可以達到部分的效果．


##Go build -ldflags

這可以在go build的時候，先設定一些變數的名稱．通常我自己比較習慣透過OS環境變數來設定，然後程式裡面再去讀取．

在你的主程式裡面，可以先定義一個變數`flagString`:


    package main
    
    import (
    	"fmt"
    )
    
    var flagString string
    
    func main() {
    	fmt.Println("This build with ldflag:", flagString)
    }

透過外在go build來設定這個變數 `go build -ldflags '-X main.flagString "test"'` 這樣你的結果會是

    >> This build with ldflag: test

這個方式可以直接設定參數，讓initialize value透過外部設定來跑． 

##Go build -tags

透過`go build -tags` 可以達到加入不同的檔案在compiling time．由於這樣，你能夠放在這個檔案裡面的東西就有很多．可以是:

- 不同系列的define (達到ifdef的效果)
- 不同的function implement (達到同一個function 在不同設定下有不同實現)

以下增加一個簡單的範例，來達到不同的build config可以載入不同的define value．

file: `debug_config.go`

    //+build debug
    
    package main
    
    var TestString string = "test debug"
    var TestString2 string = " and it will run every module with debug tag."

    func GetConfigString() string {
    	return "it is debug....."
    }

請注意: `//+build debug` 前後需要一個空行（除非你在第一行)

另外，我們也有一般的設定檔 `release_config.go`

    //+build !debug
    
    package main
    
    var TestString string = "test release"
    var TestString2 string = " and it will run every module as no debug tag."

    func GetConfigString() string {
    	return "it is release....."
    }

最後在主要的main裡面，可以直接去參考這兩個define value
file: `main.go`

    package main
    
    import (
    	"fmt"
    )
    
    func main() {
    	fmt.Println("This build is ", TestString, TestString2, GetConfigString())
    }


在這裡如果你是跑 `go build -tags debug` 那麼執行結果會是:  

    >> This build is  test debug  and it will run every module with debug tag. it is debug.....

如果跑的是 `go build`會預設去讀取`!debug`，那麼結果會是:

    >> This build is  test release  and it will run every module as no debug tag. it is release.....

可以看到他不只可以加入define參數，也可以把不同function 有著不同的implement.


##使用時機討論與心得

**使用上差異:**

- ldflags 可以加入一些參數，就跟gcc的ldflags 一樣- 
- tags 很像是 `gcc -D` 不過由於在檔案裡面要定義 `//+build XXXX`，感覺有點繁瑣． 不過由於可以以檔案來區隔，你可以加入多個定義值跟function

**ldflags 使用時機:**

個人認為可能可以拿來改變初始值得設定，或是去改變一些程式內的設定．
**比如說: **

- buffer value: 透過build 來改變buffer size，來做不同的測試與應用．
- log flag: 決定要不要印log

**tags 使用時機:**

`tags`的使用時機相當的多，列出幾個我看到的:

- debug log/encrypt : 定義debug func 然後再release/debug有不同的implement (印或是不印log)，也可以在某些狀況下開啟或是關閉encrypt來測試．
- 跨平台部分: 不同OS平台需要不同的設定與implement

其他更多的部分，可以看到在逐漸加上去．

##參考鏈結

- [http://blog.csdn.net/varding/article/details/20465311](http://blog.csdn.net/varding/article/details/20465311)
- [http://shinpei.github.io/blog/2014/10/07/use-build-constrains-or-build-tag-in-golang/](http://shinpei.github.io/blog/2014/10/07/use-build-constrains-or-build-tag-in-golang/)
- [http://stackoverflow.com/questions/15214459/how-to-properly-use-build-tags](http://stackoverflow.com/questions/15214459/how-to-properly-use-build-tags)
- [http://stackoverflow.com/questions/11354518/golang-application-auto-build-versioning](http://stackoverflow.com/questions/11354518/golang-application-auto-build-versioning)
- [http://codegangsta.io/blog/2013/07/11/practical-go-build-tags/](http://codegangsta.io/blog/2013/07/11/practical-go-build-tags/)
- [http://technosophos.com/2014/06/11/compile-time-string-in-go.html](http://technosophos.com/2014/06/11/compile-time-string-in-go.html)
- [Godoc: go commandline](https://golang.org/cmd/go/)
- [Godco: go build commands](http://golang.org/pkg/go/build/)
- [Dave Cheney: Using //+build to swtich between debug and release builds](http://dave.cheney.net/2014/09/28/using-build-to-switch-between-debug-and-release)
