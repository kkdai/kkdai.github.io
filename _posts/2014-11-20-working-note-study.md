---
layout: post
layout: post
title: "[iOS][Android]工作上的一些雜事筆記20141120"
description: ""
category: 
- 網路上好玩的事情
- ios的程式開發
- android安卓學習心得
- 學習文件
tags: []

---

許多相關雜事紀錄，盡量條列式記起來．

- [綜合]
    - 關於xmpp programming的部分
        - xmpp 的連接可能會用到不少的時間．在正式上線前，沒辦法收到訊息．
        - 所謂的正式上線．原本以為會是OnPresence，OnReadyToSend或是OnInitialize．他們的順序是 OnInit -> OnReadyToSend -> OnPresence
        - 而onPresence 還必須要有好友(buddy)才會收到
        - 真正要能送出訊息，必須直到OnPresence 
    - Linux Socket Programming
        - 似乎有遇到SIGPIPE的問題，雖然網路上都說可以使用signal(SIGPIPE,SIG_IGN)來處理斷線造成的問題．但是遇到了會被認為是仍在連線所造成的更多問題．看來還得仔細研究內容來解決問題．
        - 相關文章:
            - [https://www.ptt.cc/bbs/Linux/M.1378805552.A.A8A.html](https://www.ptt.cc/bbs/Linux/M.1378805552.A.A8A.html)
            - [http://q110185.blogspot.tw/2009/01/linuxsigpipe-handle.html](http://q110185.blogspot.tw/2009/01/linuxsigpipe-handle.html)
            - [http://www.cppblog.com/elva/archive/2008/09/10/61544.html](http://www.cppblog.com/elva/archive/2008/09/10/61544.html)
- [iOS] 
    - HealthKit的請求權限(requestAuthorizationToShareTypes)必須在一開始就要要求．
        - 如果有需要新增，或是減少，只是重新執行App是沒有用的．需要重新安裝才會跑到正確的集合．
        - 它類似GameCenter一樣，當你試著移除App的時候．會詢問你要不要把相關的數據全部移除．
    -  WatchKit也放出來了，幾件事情值得記起來．
        - WatchKit類似App Extension(Today Widget)類似，可能需要手機App作為互動本體．
- [MacOSX]
    - 本來想試著找找MacOSX上面的FileMerge工具(個人習慣使用[Araxis Merge](http://www.araxis.com/merge/))．但是太貴了，後來用習慣Xcode裡面的FileMerge也就放棄繼續找了．
         
- [MSFT]
    - 微軟在2014/11/13第二天的Connect Developer Event 裡面，做了[重大的宣布](http://weblogs.asp.net/scottgu/announcing-open-source-of-net-core-framework-net-core-distribution-for-linux-osx-and-free-visual-studio-community-edition)：
        - 開放.Net Framework 原始碼，範圍包括CLR，JIT(Just-In-Time Compiler)，GC與.NET基本的一些Framework．
        - 目前開放只有XPATH與Collection，喜愛用.NET裡面XML物件的可以去看看．[hub.com/dotnet/corefx](hub.com/dotnet/corefx)
    - 微軟也宣布可以內建支援Docker，並且有放出原始碼給大家用．[這篇文章](https://ahmetalpbalkan.com/blog/compiling-docker-cli-on-windows/)有更多資訊．                                                
     

**相關資料:**

- 關於HealthKit中文說明
    - [https://www.apple.com/tw/ios/whats-new/health/](https://www.apple.com/tw/ios/whats-new/health/)
- 關於MSFT開放原始碼與相關變更的網頁
    - [http://weblogs.asp.net/scottgu/announcing-open-source-of-net-core-framework-net-core-distribution-for-linux-osx-and-free-visual-studio-community-edition](http://weblogs.asp.net/scottgu/announcing-open-source-of-net-core-framework-net-core-distribution-for-linux-osx-and-free-visual-studio-community-edition)
- Apple Developer[Need apple account] - App Extension (Widget, Today, Share ...)    
    - [https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/NotificationCenter.html](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/NotificationCenter.html) 
- How to compiler Docker in Windows    
    - [https://ahmetalpbalkan.com/blog/compiling-docker-cli-on-windows/](https://ahmetalpbalkan.com/blog/compiling-docker-cli-on-windows/)
- About SIGPIE
    - [https://www.ptt.cc/bbs/Linux/M.1378805552.A.A8A.html](https://www.ptt.cc/bbs/Linux/M.1378805552.A.A8A.html)
    - [http://q110185.blogspot.tw/2009/01/linuxsigpipe-handle.html](http://q110185.blogspot.tw/2009/01/linuxsigpipe-handle.html)
    - [http://www.cppblog.com/elva/archive/2008/09/10/61544.html](http://www.cppblog.com/elva/archive/2008/09/10/61544.html)
         
