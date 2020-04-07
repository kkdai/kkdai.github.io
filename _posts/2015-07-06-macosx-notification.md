---
layout: post
layout: post
title: "[Golang]關於MacOSX Desktop Notification(桌面提醒)"
description: ""
category: 
- golang
tags: []

---

![image](http://www.adambarth.com/experimental/crx/docs/images/notification-mac.png)

##前言:

想要寫一個小工具，可以把一些http link傳給下指令的人．卻又苦於一般CLI無法直接顯示Hyperlink．想說參考了一下[gomon](https://github.com/c9s/gomon)，不過卻發現原來MacOSX的桌面通知有不少方式可以顯示．

##可以發送MacOSX Desktop Notification的方式:


#### [免費的] 透過Apple Script來發送

[Apple Script](https://developer.apple.com/library/mac/documentation/AppleScript/Conceptual/AppleScriptLangGuide/reference/ASLR_cmds.html) 提供一個簡單的方式，可以在任何Application 或是 CLI的來發送通知．

不過他有一些缺點:

- 不支援HTTP LINK
- 不支援自訂圖片
- 無法自訂標題或是ICON

這裏有一個Golang 的小工具，有將Apple Script 整理過[https://github.com/everdev/mack](https://github.com/everdev/mack)


#### [免費的] 透過terminal-notifier

[terminal-notifier](https://github.com/julienXX/terminal-notifier)是一個由ObjectiveC與Ruby構成的小App，可以透過Homebrew來安裝．

雖然可以開啟URL，但是還是有以下的缺點:

- 無法自訂圖片
- 不支援ICON




#### [付費] Growl

[Growl](http://growl.info/)算是相當知名的Notification App，提供相當多的[API給使用者](http://www.growlforwindows.com/gfw/help/gntp.aspx)使用．

這裏列出幾個Go的相關工具:

- [go-gntp](https://github.com/mattn/go-gntp)
    - 透過Golang來呼叫Growl 的APIs
- [Gomon](https://github.com/c9s/gomon)
    - 會幫你檢查source code並且回報給你．不過也是透過Growl來做的．
          
