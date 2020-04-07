---
layout: post
layout: post
title: "[Android學習]心血來潮把Android SDK更新(ADT 23)所帶來的問題與解決"
description: ""
category: 
- android安卓學習心得
tags: []

---

![image](http://www.evanlin.com/images/2014/Android_ICON.jpg)

**前言：**

週末前剛好心血來潮更新了Android SDK，也看到了ADT有了23版，並且當然也有下載最新的Android L 的SDK．想不到下載完之後造成Eclipse完全無法使用，利用了週末的時間好好的把系統整理了一次．(決定重灌，並且也要重新安裝gradle) 

接下來就記錄一下，那些部分有遇到問題．希望能幫助到一樣遇到問題的你．以下這篇文章有更清楚地解釋[出現了22.6更換到23版的錯誤](http://blog.mosil.biz/2014/06/android-sdk-tools-update-part-3/)．




**錯誤與修復:**


![image](http://i.stack.imgur.com/atObk.png)


- 有關更新到Android SDK 23的錯誤
    - 錯誤狀況:
        - "This Android SDK requires Android Developer Toolkit version 23.0.0 ..."並且重開也沒有效過．當然Check for update 也沒出現任何可以更新
    - 解決方法:
        - 根據一些[網友的討論](http://stackoverflow.com/questions/24525595/this-android-sdk-requires-android-developer-toolkit-version-23-0-0-or-above)，似乎在[Install New Software]-> update SDK to 23.0
        - 但是我都會出現更新的dependency 出錯，根據[這篇文章](http://blog.mosil.biz/2014/06/android-sdk-tools-update-part-3/)似乎從2014的六月到現在都還沒解決．所以我還是決定重灌Android Developer Toolkit

- 有關更新到ADT 23之後，ADB 無法順利的執行(如果你有使用Genymotion 23.1之前的版本的話)
    - 錯誤狀況:
        - Genymotion 模擬器執行之後，Eclipse一直無法正確的編譯程式．出現 "The connection to adb is down, and a severe error has occured"
        - 到 ANDROID_SDK/sdk/platform-tools/adb 去跑 ./adb kill-server 與 ./adb start-server 也無法成功．
    - 解決方法:
        - 這邊有兩個解決方法，一個是把你的Genymotion更新到23.1
        - 另外一個就是依照以下[這篇文章](http://stackoverflow.com/questions/26431972/android-studio-lollipop-adb-genymotion-issues-devices-wont-show-up-adb)去改變genymotion的設定不使用自己的Adb而使用Android SDK的adb
                                    
        
        
**參考資料:**
        
- About stackoverflow in ADT 23 error
    - [http://stackoverflow.com/questions/24525595/this-android-sdk-requires-android-developer-toolkit-version-23-0-0-or-above](http://stackoverflow.com/questions/24525595/this-android-sdk-requires-android-developer-toolkit-version-23-0-0-or-above)                           
- In Mozil blog discussion about ADT 23 error
    - [http://blog.mosil.biz/2014/06/android-sdk-tools-update-part-3/](http://blog.mosil.biz/2014/06/android-sdk-tools-update-part-3/)        
- Genymotion Adb error
    - [http://stackoverflow.com/questions/26431972/android-studio-lollipop-adb-genymotion-issues-devices-wont-show-up-adb](http://stackoverflow.com/questions/26431972/android-studio-lollipop-adb-genymotion-issues-devices-wont-show-up-adb)         
