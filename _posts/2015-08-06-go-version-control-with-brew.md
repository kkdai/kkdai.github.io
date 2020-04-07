---
layout: post
layout: post
title: "[Golang]透過brew來做Golang的版本切換"
description: ""
category: 
- golang
tags: []

---


##  前言:

本來想在Mac OSX上面使用[gvm](https://github.com/moovweb/gvm)來管理Golang 的版本切換，主要是因為想切到 Go 1.5 beta來玩玩新東西．

不過昨天去GTG聚會之後，學到`brew switch`之後，覺得整個很威啊... 這裏把幾個筆記記錄一下:

###指令集:

####  使用brew安裝Golang

        //把brew的fomulae更新到最新版本
        sudo brew update

        //使用brew安裝Go
        brew install go

#### 更新版本

        //拿目前最新版本 Go 1.5 beta3 舉例
        brew upgrade go -v=1.5beta3 --devel
        
        //注意.. 就的版本 Go 1.4.2  不會從你的brew位置移除
        //可以查詢
        brew info go:
        
        
        //目前系統有安裝兩個版本.... 使用的是1.4.2
        go: stable 1.4.2 (bottled), devel 1.5beta3, HEAD
        Go programming environment
        https://golang.org
        /usr/local/Cellar/go/1.4.2 (4584 files, 276M) *
            Poured from bottle
        /usr/local/Cellar/go/1.5beta3 (5357 files, 277M)
            Built from source
        From: https://github.com/Homebrew/homebrew/blob/master/Library/Formula/go.rb
        
#### 切換版本

        //使用brew switch來切換        
        brew switch go 1.5beta3
        
        //螢幕顯示
        Cleaning /usr/local/Cellar/go/1.2.1
        Cleaning /usr/local/Cellar/go/1.4.2
        Cleaning /usr/local/Cellar/go/1.5beta3
        3 links created for /usr/local/Cellar/go/1.5beta3
        
        
        //如此就可以切換.... 不過檔案不會移除
        
#### 反安裝版本

        //使用brew uninstall
        brew uninstall go
        
        //只會移除目前使用的版本... 不過由於原本目錄沒有指向新的，所以功能都無法使用
        //將目前使用的版本切換到 1.4.2
        brew switch go 1.4.2 

## 可能會發生的錯誤

切換Go版本，然後跑`go build`可能會發生以下的錯誤:

        package .
        	imports runtime: C source files not allowed when not using cgo or SWIG: atomic_amd64x.c defs.c float.c heapdump.c lfstack.c malloc.c mcache.c mcentral.c mem_darwin.c mfixalloc.c mgc0.c mheap.c msize.c os_darwin.c panic.c parfor.c proc.c runtime.c signal.c signal_amd64x.c signal_unix.c stack.c string.c sys_x86.c
                
#### 解決方式(solution for "C source files not allowed when not using cgo or SWIG"):

這裏提供我的解決方式:

1. 刪除 `%GOPATH%`下的`pkg`資料夾
2. 重新開啟 console 或是確認 `go env`有讀到正確 go 1.5的設定．                

##心得

[gvm](https://github.com/moovweb/gvm)主要是給Ubuntu(linux-like)來管理Golang版本套件的．既然是Mac OSX就要用最好使用的[homebrew](http://brew.sh/)．

搞定 `brew switch` 切換Go 版本真的是很方便... 不過c-shared  MAC也沒有  想玩還是有點麻煩...   還是在Win上面弄docker玩...
