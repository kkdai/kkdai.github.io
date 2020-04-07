---
layout: post
layout: post
title: "[iOS][粉絲相簿更新]更新App版本到XCode6"
description: ""
category: 
- ios的程式開發
- app-粉絲相簿
tags: []

---

**前言:**

雖然我不算是專業App開發者，但是有了新版本的XCode更新還是會更新一下．這裡也記錄一些更新的過程中可能會有的問題．

**過程筆記：**

- 是否要保留XCode5，該如何保留？
    -  要不要保留xcode5？
        - 這個見仁見智，不過短時間Apple不會阻擋xcode所submit的App之前．如果不想踩到太多地雷，或是企業外包App可能可以先保留起來．
    - 如何保留？ 
        - 這個動作必須要在App更新之前．
        - 建議把XCode從 Application 裡面複製出來．如果只是改名字是沒有用的．
        
- 模擬器發生錯誤，Simulator Error: "An error was encountered while running (Domain = FBSOpenApplicationErrorDomain, Code = 4)"
    - Fixed: iOS Simulator -> Reset Content And Settings…
    - 可以參考這篇: [http://qiita.com/tajihiro/items/f6f50b56162c93d25c90](http://qiita.com/tajihiro/items/f6f50b56162c93d25c90)

- App Package的大小從5.6MB-> 3.1MB
    - 原因我還沒查清楚，只知道大小縮小接近一半．這倒是可以好好研究看看．
    - 看起來原因是出在Provisioning 不一樣的原因，不過這個會造成大小差異？倒也是很奇怪．
    



    


        
    
