---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-03 11:17:01+00:00
slug: golangmac%e6%ba%96%e5%82%99golang%e9%96%8b%e7%99%bc%e7%92%b0%e5%a2%83
title: '[Golang][Mac]準備Golang開發環境'
wordpress_id: 1436
categories:
- Golang
- 學習文件
---

基本上其實只要參考 [Simple Patrick:My Go Development Environment](http://yinghau76.github.io/2013/12/14/my-go-development-environment/)那篇應該就可以．只是我這邊記錄一下我遇到的問題，不知道其他人會不會遇到．






  * brew install go 會成功但是一直跳出要 brew link go，然後如果你打brew link go就會跳出permission不足．找不到答案，於是去[官方網站抓下來裝](http://golang.org/dl/)


  * Sublime text2跟GoSublime 是可以完美結合．DashDoc 也可以正常用，只是要先裝Dash App Store裝好．


  * 弄好了這一切，其實看起來是完美了．但是就Golang的特性，沒有debugger似乎很難做更一步地解決問題．



    * 於是去找了這篇文章裝LiteIDE然後跑Golang [http://www.goinggo.net/2013/06/installing-go-gocode-gdb-and-liteide.html](http://www.goinggo.net/2013/06/installing-go-gocode-gdb-and-liteide.html) 幾個大方向



      * 抓GDB安裝


      * 建立一個codesign


      * 把GDB 簽署當作可以信任的執行檔
      * 記得要codesigbn gdb `codesign -f -s  "gdb-cert" /usr/local/bin/gdb`
	      * [更新2016/02/05] 記得每次gdb更新都要跑....



    * 一開始[LiteIDE](https://github.com/visualfc/liteide)可能會出現go bin not found 的錯誤，需要把設定重新跑好


    * [View] -> [Edit Environment]  (我的可以參考一下)



      * 

    
    GOROOT=/usr/local/go



    
    GOBIN=/usr/local/go/bin/



    
    GOARCH=amd64



    
    GOOS=darwin



    
    CGO_ENABLED=1



    
    PATH=$GOROOT/bin:$PATH



    
    LITEIDE_GDB=/usr/local/bin/gdb



    
    LITEIDE_MAKE=make



    
    LITEIDE_TERM=/usr/bin/open



    
    LITEIDE_TERMARGS=-aTerminal



    
    LITEIDE_EXEC=/usr/X11R6/bin/xterm



    
    LITEIDE_EXECOPT=-e






    * 不過text highlighter 跟 dash無法整合有點痛苦，可能會拿LiteIDE作為debug 的工作．



  * 其實Sublime Text2 也是有 GDB support 的，繼續得研究看看




 
