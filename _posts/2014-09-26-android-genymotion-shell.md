---
layout: post
layout: post
title: "[Moocs]Programming Mobil Aplication on Android Platform (1)"
description: ""
category: 
- 網路課程筆記
- android安卓學習心得

tags: []

---


![image](../images/2014/Android_ICON.jpg)

**前言:**

這本來是Moocs上面一系列關於Java/Android課程Mobile Cloud Computing with Android的第二門課程．由於最近我把第二階段的課上完了．也在想說要不要學一下．先開始follow看看...

其實對於Android 我還是處於不是很熟悉的狀態．雖然當初已經把一些簡單的相機功能有寫出來．但是整個還是架構不熟悉．也順便來學習一下．

**關於環境使用:**

關於Emulator的Shell使用，由於我使用的是Genymotion．稍微玩了一下，發現command幾乎都不太一樣．

-  Genyshell 必須在Genymotion模擬器跑起來之後再用
    - genymotion capabilities 可以查詢所有支援的指令
        - 就算你查到camera/accleronmeter可以使用，但是其實因為是模擬器．其實是不能用的．
    - battery getlevel/setlevel 來設定電源目前使用量 
    - gps setaltitude/setlatitude 可以來設定目前gps地點資訊
- Emulator Call
    - 這個部分使用AVD(Android Virtual Device)可以，但是我還沒找到如何使用Genymotion來做．
- DDMS Perspective
    - 這個部分我比較沒有在使用，不過這次的教學也會清楚地教導如何使用DDMS．主要在Hierachy View 還有 Methond profiling．



**關於Activity Lifecycle**

筆記一些我比較沒有注意的部分:

- OnResume 發生的時間點是最多的，基本上就是Activity 出現前就會發生．所以以下情況都會發生:
    - 第一次執行OnCreate之後
    - 離開App回來之後
    - OnPause與OnRestart的返回
- OnPause 發生在準備要離開Activity的時候，緊接著是OnStop會收到
- OnStop 會出現在 OnPause之後，所以不一定會被呼叫到，如果user使用kill app．這時候只會有OnPause
- 旋轉手機的時候，OnCreate，OnStart跟OnResume都會發生
- 
其他更多可以在這裡看到 [http://developer.android.com/training/basics/activity-lifecycle/index.html](http://developer.android.com/training/basics/activity-lifecycle/index.html)
    

        
        
