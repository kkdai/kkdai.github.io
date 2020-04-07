---
layout: post
layout: post
title: "[iOS][Python]手機掃描條碼下載相關的登入帳號數據"
description: "[iOS][Python]手機掃描條碼下載相關的登入帳號數據"
category: 
- django
- paas
- ios的程式開發

tags: []

---

![image](https://www.python.org/static/img/python-logo.png)

記錄一些關於開發這個的一些心得，主要就是手機會透過掃QR-code的方式，來讀取一些帳號資料讓手機可以登錄到相關的服務．
也就是說， 其實手機不用再透過手動的輸入帳號與密碼．而是使用QR-Code掃描．

所以流程就是:

- 手機打開App
- 掃描QR-code
- 連接到某個Json網頁
- 分析json 資料變成登入帳號密碼
- 登入某些服務．

 
剛好遇到我切換到xcode6.1，這裡有一些筆記：

- Xcode 6.1 GM似乎不太穩定，遇到問題有以下兩個:
    - Random crash: 而且Mac App會收起來(?)真百思不得其解．
        - 主要發生crash 在用滑鼠去點執行或是停止，但是用快捷鍵 Cmd + R 跟 Cmd + . 是沒問題的．
    - Storyboard 物件，變更大小的時候，某些物件會消失．
- 關於使用的PAAS Server
    - 之前架設django的資料庫管理，當然還沒有連到外在的資料庫，而是使用SQLlite．所以當服務被重啟（PAAS每個30分鐘到一個小時會把你的服務設定維修棉．需要重啟大概要30 sec的啟動時間）． 服務重啟～檔案會恢復當初上傳的狀態．
    - 暫時解決方式，先把需要測試的資料上傳SQLlite檔案上去．
    - 之後會把資料移到MongoDB HQ
- XCode 6.1使用到不知道 X64的toolkit
    - 會出現錯誤 :"Undefined symbols for architecture arm64" [http://stackoverflow.com/questions/19213782/undefined-symbols-for-architecture-arm64](http://stackoverflow.com/questions/19213782/undefined-symbols-for-architecture-arm64)
    - 建議處理方式: 需要到以下地方修改
        - "Build Setting-> Architectures"那邊去修改，先看看原先的設定是什麼，由於我使用 Zbar code reader [http://zbar.sourceforge.net/](http://zbar.sourceforge.net/)，所以他的用法是“$(ARCHS_STANDARD_32_BIT)”
        - "Build Setting-> Valid Architectures" 改成 "arm64, armv7 armv7s"
    
    
**相關資料:**              

- 掃描許多qrcode或是barcode 的toolkit  Zbarcode
    -  [http://zbar.sourceforge.net/](http://zbar.sourceforge.net/)
- 解決沒有x64 module編譯錯誤
    - [http://stackoverflow.com/questions/19213782/undefined-symbols-for-architecture-arm64](http://stackoverflow.com/questions/19213782/undefined-symbols-for-architecture-arm64)
- 受不了xocde 6.1? 這裏有 xcode 5.1
    - [http://stackoverflow.com/questions/10335747/how-to-download-xcode-4-5-6-and-get-the-dmg-file](http://stackoverflow.com/questions/10335747/how-to-download-xcode-4-5-6-and-get-the-dmg-file)          
- 如果快速把iphone storyboard搬到iPad上？ 但是我反之似乎不成功
    - [http://stackoverflow.com/questions/8465769/converting-storyboard-from-iphone-to-ipad](http://stackoverflow.com/questions/8465769/converting-storyboard-from-iphone-to-ipad)
- Django MongoDB 連接方式
    - [https://django-mongodb-engine.readthedocs.org/en/latest/topics/setup.html](https://django-mongodb-engine.readthedocs.org/en/latest/topics/setup.html)
- MongoDB HQ(改名為compose.io) 免費的線上MongoDB host 服務
    - [https://www.compose.io/](https://www.compose.io/)               

