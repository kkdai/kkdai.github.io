---
layout: post
layout: post
title: "[Mooc][Android]Programming Mobile Aplication on Android Platform(week5) - Notification and Threads"
description: ""
category: 
- android安卓學習心得
- 網路課程筆記
 
tags: []

---

![image](http://www.evanlin.com/images/2014/Android_ICON.jpg)

**前言:**

來到第五週，其實作業已經越來越花時間．需要好好把握時間趕快做完才行．

**筆記：**

- 關於 Broadcast:
    - 可以是有順序的(利用setPriority來設定)，其中Priority 越高順序越前面．
    - 一個broadcast 可以同時被多個broadcast receiver 接收．
    - 其中一個Broadcast receiver 可以透過abortBroadcast 來強迫其他receiver來收取broadcast．
    - 關於sticky broadcast 比較需要注意的是:
        - 接收 sticky broadcast可以收到在註冊前發生的變動．            
- 關於 Threads:
    - Java的thread 建立之後不會馬上執行．需要跑start()
    - 背景的thread無法執行UI的相關動作．(這個地方跟 WinApp 跟 iOS App都一樣)
        - 需要執行要跑 runOnUIThread()
        - 或是每一個UI物件都有自己的View的post函式．View.post(Runnable action)    

![image](http://static.squarespace.com/static/506e28cee4b04973cff61716/t/5174622ce4b010aac9554b43/1366581806849/Android%20AsyncTask%20Example%20Chart.png?format=1500w)

- 關於AsyncTask:
    - 流程:
        - OnPreExecute -> doInBackground() -> publishProgress() -> OnPostExecute()
        - 其中如同上圖，有相對應的UI thread method OnProgressUpdate()最後 ObPostExecute() 也可以執行UI的更新．
- 關於Alarm:
    - 註冊後，就送機器進入睡眠模式一樣可以收到．（除非關機)
    - 要取用Alarm需要使用 getSystemService(Context.ALARM_SERVICE) 來取得．
    - Kitkat (API 19) 之後，Alarm的使用變成inexact，也就是說他會盡量把幾個差不多時間的Alarm收集起來一起發送，即時他們時間有些許的差異．透過這個方式來節省電源的使用． (詳細可以參考[這段](http://developer.android.com/reference/android/app/AlarmManager.html))

- 關於Networking: (Socket, JSON, URLRequest)
    -  要做網路溝通使用 [HttpURLConnection](http://developer.android.com/reference/java/net/HttpURLConnection.html) 而不要使用 [AndroidHttpClient](http://developer.android.com/reference/android/net/http/AndroidHttpClient.html) 跟 DefaultHttpClient，由於[Android team在 Android 2.3之後就不太積極開發](http://android-developers.blogspot.tw/2011/09/androids-http-clients.html)． 
    - JSON的取用方式:
        - 要先用ResponseHandler 接回 client的 response 之後再逐一分解．
        - data array 可以用 JSONObject.getJSONArray
        - 每個mapping key/value 可以使用JSONObject.get(key)
    - XML的取用方式：
        - 主要有三種存取的方式: (DOM, SAX, PULL)
        - 其中記憶體消耗最高的是DOM，因為讀取的時候會把所有的XML樹狀資料全部載入．
        - SAX與PULL都是屬於event callback的方式，而PULL提供更多的彈性可以逐一的存取，並且可以控制event的結束．
                

**參考資料:**

- 關於broadcast的一些整理
    - [http://blog.csdn.net/way_ping_li/article/details/8016688](http://blog.csdn.net/way_ping_li/article/details/8016688)
- Broadcast 種類整理:
    - [http://lazyjames.logdown.com/posts/147044-broadcast-receiver](http://lazyjames.logdown.com/posts/147044-broadcast-receiver)
         
- 關於Threads Oracle上面的Toturial
    - [http://docs.oracle.com/javase/tutorial/essential/concurrency/threads.html](http://docs.oracle.com/javase/tutorial/essential/concurrency/threads.html)
- O'reilly 有專門的書籍介紹 Java Threads
    - [http://www.oreilly.com.tw/product_java.php?id=a159](http://www.oreilly.com.tw/product_java.php?id=a159)
- [Android] Android 官方document 
    - [http://developer.android.com/reference/android/content/Context.html#registerReceiver(android.content.BroadcastReceiver](http://developer.android.com/reference/android/content/Context.html#registerReceiver(android.content.BroadcastReceiver) 
- [Android]關於AlarmManager的使用                    
    - [http://developer.android.com/reference/android/app/AlarmManager.html](http://developer.android.com/reference/android/app/AlarmManager.html) 
- 關於 HttpRequest 的挑選與基礎教學
    - [http://android-developers.blogspot.tw/2011/09/androids-http-clients.html](http://android-developers.blogspot.tw/2011/09/androids-http-clients.html)  
- 關於 Android XML 分析方式的講解(DOM, SAX, pull)       
    - [http://www.cnblogs.com/xiaoluo501395377/p/3444744.html](http://www.cnblogs.com/xiaoluo501395377/p/3444744.html)
    - [http://www.cnblogs.com/JerryWang1991/archive/2012/02/24/2365507.html](http://www.cnblogs.com/JerryWang1991/archive/2012/02/24/2365507.html) 
