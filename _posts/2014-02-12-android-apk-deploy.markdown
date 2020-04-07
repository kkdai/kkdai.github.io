---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-02-12 23:38:04+00:00
slug: android%e4%b8%80%e4%ba%9b%e9%97%9c%e6%96%bcandroid-apk%e9%83%a8%e7%bd%b2%e5%b0%8f%e7%ad%86%e8%a8%98
title: '[Android]一些關於android apk部署小筆記'
wordpress_id: 1331
categories:
- Android安卓學習心得
- 學習文件
---

記錄一下，最近的一些小筆記






  * 關於Android App安裝到device上面：(在Win8上面)



    * 如果要註冊非官方授權的device（比如說大陸貼牌device)


    * 需要去[裝置管理員]把裝置例項路徑中把VID抄起來(ex:1D06)


    * 把資料抄到 ADT裡面的 SDKExtragoogleusb_driverandroid_winusb.inf


    * 滑鼠移到右下角，選擇[變更電腦設定]


    * [更新與復原]->[復原]->[進階啟動] 來重開機


    * 將裝置管理員裡面無法認識的裝置驅動程式選到 剛剛那個檔案android_winusb.inf


    * 如此一來可以把裝置識別出來為 [Android通用裝置]


    * 到 user_data/.android/adb_usb.ini



      * 注意：  如果因為android SDK 更新這個動作需要重新做一次



    * 把裝置新增上去起來 比如 0x1D06


    * 重啟Eclipse~就能正常讀取



  * 用Eclipse 把App裝到android裝置上面(手機/平板)



    * 使用Adb command line



      * [File]->[Export] 成 api


      * 這時候會設定你的私密金鑰


      * 輸出後使用 adb來安裝


      * adb install helloworld.apk


      * 可以先用 adb devices 確認裝置有讀取到


      * 或是使用 adb kill-server 來強迫重新讀取裝置




  * 開啟Android 專案出現 [invalid project description]



    * 這時候不能使用 Add Android using exist code


    * using  [import]->[general]->[Existing Project into workspace]來載入



  * **相關資料:**



    * How to install android app into android device: [http://readandplay.pixnet.net/blog/post/139916128](http://readandplay.pixnet.net/blog/post/139916128)


    * About project loading error: [http://www.itkee.com/developer/detail-725.html](http://www.itkee.com/developer/detail-725.html)


    * Camera sample code: [http://simonxanderandroid.blogspot.tw/2011/02/android-camera.html](http://simonxanderandroid.blogspot.tw/2011/02/android-camera.html)


    * Using full size camera surface [http://stackoverflow.com/questions/11278253/stretching-camera-preview-to-full-screen-in-android](http://stackoverflow.com/questions/11278253/stretching-camera-preview-to-full-screen-in-android)



