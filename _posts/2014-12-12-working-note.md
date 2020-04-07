---
layout: post
layout: post
title: "[iOS][Android][Python]工作上的一些雜事筆記20141212"
description: ""
category: 
- 網路上好玩的事情
- ios的程式開發
- android安卓學習心得
- python
- 學習文件
tags: []

---

**前言:**

本週都在Intel上課，大部份內容屬於NDA．只能找空閒時間看看其他的部分．

**筆記:**

- [Mac OSX]一些常用的工具
    - 想要尋找Mac上面類似pietty一樣好用的軟體，卻忘記terminal 本身就有一樣的功能．更何況我有裝iTerm2
        - [指令] ssh -l username -p portnumber host
        - 參考：
            - [25個SSH指令](http://max-linux-space.blogspot.tw/2011/07/25ssh-commands.html)

    - 除了想要尋找類似pietty一樣的軟體外，也想要尋找可以一次完成SSH跟SFTP的軟體
        - [Fugu](http://rsug.itd.umich.edu/software/fugu/) 就是我想找的．                 


- [Android][iOS] PhoneGap Beta Release
    - 這個東西可以幫助你開發Web App並且可以在手機(模擬器)上面瀏覽．
    - [http://phonegap.com/blog/2014/12/11/phonegap-desktop-app-beta/](http://phonegap.com/blog/2014/12/11/phonegap-desktop-app-beta/)

- [Android] Android Studio 1.0 official release
    - 剛剛痛苦的把Android Project轉換成Android 1.0RC2之後，official release出來了．找時間好好來看看如何使用．
    - [https://developer.android.com/sdk/index.html](https://developer.android.com/sdk/index.html)

- [iOS] Parse支援Crash Log，可以考慮要不要加進去自己的App．不過我App用的是Parse 1.2.19現在已經到1.6了．
    - 相關介紹網址  [http://goo.gl/70jIEb](http://goo.gl/70jIEb)         
    - 這一篇有介紹一些toolkit可以使用
        [http://www.raywenderlich.com/33669/overview-of-ios-crash-reporting-tools-part-1](http://www.raywenderlich.com/33669/overview-of-ios-crash-reporting-tools-part-1)
        - TestFlight也是很多人推薦 [https://www.testflightapp.com/](https://www.testflightapp.com/)
        - 當然挺多人推薦的HockeyApp [http://hockeyapp.net/features/](http://hockeyapp.net/features/)

- [IaaS][PaaS] 在Azure上面架設VM(Virtual Machine)很方便．但是對外的port要透過設定不然預設都是關閉的．這件事情讓我很困擾．
    - 問題:
        - 建立了伺服器與服務，但是對外都無法找到相對應的服務
        - 內部自己用curl或是其他client app可以找到相對應的服務
    - 解決方式:
        - 在Endpoint得加入你要開放給外面的port
        - 甚至可以做一個簡單的port 修改，比如說把xmpp 從5222改到80，裡面的80 Apache改到8080         
    - 此外，現在Azure是有Preview的Manager Dashboard，東西有變多了．     
        
