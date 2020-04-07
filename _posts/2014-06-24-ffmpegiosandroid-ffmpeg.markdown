---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-06-24 10:19:16+00:00
slug: ffmpegiosandroid-%e5%a6%82%e4%bd%95%e5%9c%a8iosandroid%e4%b8%8a%e9%9d%a2%e4%bd%bf%e7%94%a8-ffmpeg
title: '[ffmpeg][ios][Android] 如何在iOS/Android上面使用 ffmpeg'
wordpress_id: 1425
categories:
- Android安卓學習心得
- ffmpeg
- iOS的程式開發
---

自從在Windows完成 live555(OpenRTSP)與 ffmpeg之後，就很想把它整理好之後放到iOS與Android上面．  
不過尋找了一下，發現大部份人在iOS與Android上面都是直接使用 ffmpeg來播放 RTSP的資料．  
不過其實Android  4.0之後其實 Videoview就已經支援RTSP的播放，那麼為何還需要ffmpeg呢?  
—>  利用 VideoView 會有10 秒的延遲，如果想要把延遲減少比較好的方式就不要用build-in player而是使用ffmpeg來播放檔案或是RTSP streaming




所以把如何使用的部分做了一下整理，分別有iOS與Android的部分如下:


<!--more-->



  * **iOS上如何使用ffmpeg**



    * [http://www.takobear.tw/201702608526356260322804024687/opensource-ffmpeg-21-for-ios-xcode-51-os-x-1092?](http://www.takobear.tw/201702608526356260322804024687/opensource-ffmpeg-21-for-ios-xcode-51-os-x-1092?)


    * 這篇文章已經把ffmpeg去整合在xcode 5.1.1裡面其實就超好用，實際演練 iOS6/iOS7都是可以跑的． 簡單地講在iOS上面使用 ffmpeg流程如下:



      * 利用clang 把ffmpeg 編譯起來， libavXXX.a 


      * 在iOS專案裡面加入各個a 並且把header搬進來


      * 如果有裝Xcode 6 beta，千萬記得要把command line tool 改掉．測試過xcde6 beta1 是會有問題的．



    * 更多參考:



      * [http://www.takobear.tw/201702608526356260322804024687/ffmpeg-ios-61](http://www.takobear.tw/201702608526356260322804024687/opensource-ffmpeg-21-for-ios-xcode-51-os-x-1092?)


      * [https://github.com/BigHillSoftware/QTFFmpeg](http://www.takobear.tw/201702608526356260322804024687/opensource-ffmpeg-21-for-ios-xcode-51-os-x-1092?)




  * **Android上如何使用ffmpeg**



    * 由於Windows 我一直無法正確把ffmpeg編譯起來這裡只講Mac版本



      * 裝[ccache](http://ccache.samba.org/)


      * 安裝[Android NDK for Mac](https://developer.android.com/tools/sdk/ndk/index.html) 我是裝r9b (OSX 10.9.3)


      * 設定系統參數



        * touch ~/.bash_profile; open ~/.bash_profile


        * 增加一個新的 export ANDROID_NDK=你解開NDK的位置


        * 記得把它加入你的PATH會更簡單



      * 測試NDK



        * 移動目錄到 /home/android-ndk-r5b/samples/hello-jni/


        * 執行 ndk-build



      * 準備build ffmpeg



        * 下載script [https://github.com/yixia/FFmpeg-Android](https://github.com/yixia/FFmpeg-Android)


        * 執行./FFmpeg-Android.sh


        * 查看 ./build/ffmpeg 下是否有 armv6/armv7/neon/vfp 目錄並且確認是否有libffmpeg.so



      * 建立demo App



        * 先寫一個可以播放rtsp的player(這裡是[基本播放](http://imax-live.blogspot.tw/2012/12/videoview.html))，不過要有Android可以播的倒是很麻煩... (可以先試試看[mp4](http://techslides.com/demos/sample-videos/small.mp4))



      * 加上JNI的支援(這裡由於我是初學，所以搞得有點久，可能會記錄詳細一點)：



        * 新增一個jni的資料夾


        * 新增檔案 Android.mk 還有ffmpeg-jni.c 內容可以參考[這篇文章](http://blog.changyy.org/2013/02/android-ffmpeg.html)



          * 要注意ffmpeg-jni.c 裡面的函數名稱有 Android Project name 與package name要對應到你的設定



        * 利用 command line 的NDK來編譯JNI (也就是 裡用command line在project 目錄上 打ndk-build)


        * 這時候會產生 libs 裡面的檔案，重新clean與rebuild Android App就可以看到正確的鏈結．



      * 如果想要把JNI加入Eclipse的自動編譯(參考[這篇文章](http://blog.changyy.org/2011/07/android-android-ndk-native-development.html))，步驟如下:



        * 在專案上面選[Properties] -> Builders ->New-> Program


        * 在[Main] Tab裡面



          * Name: 任意


          * Location: 選取NDK安裝目錄


          * Working Directory: “${workspace_loc:${project_path}}” (建議照貼，不然會出現error: Please define the NDK_PROJECT_PATH variable to point to it.



        * [Refresh] Tab要選擇



          * [Specific Resource] 然後位址指定 libs (也就是產出.so 的位置）



        * [Build Option] Tab要勾選以下，就可以透過 clean 來啟動 ndk build 動作。



          * [Allocation in Console]


          * [Launch in background]


          * [After a clean ]


          * [During a manual builds ]


          * [Specific Working Set] -> 選擇JNI 資料夾



        * 這樣可以測試一下去修改 jni.c 的檔案，然後看輸出．


        * 範例程式 [https://github.com/kkdai/AndroidRTSPPlayer](https://github.com/kkdai/AndroidRTSPPlayer)



      * 參考:



        * [android FFmpeg的编译（Mac版）](http://www.yoyong.com/archives/778) [可以看一下如何在Mac上面編輯]


        * [http://blog.changyy.org/2011/07/android-windows-android-ndk-native.html](http://blog.changyy.org/2011/07/android-windows-android-ndk-native.html)


        * [http://blog.changyy.org/2011/07/android-ndk-jni-c-call-java.html](http://blog.changyy.org/2011/07/android-ndk-jni-c-call-java.html)


        * [http://mobilepearls.com/labs/ndk-builder-in-eclipse/](http://mobilepearls.com/labs/ndk-builder-in-eclipse/)


        * [http://www.pupuliao.info/2013/05/%E5%9C%A8eclipse%E9%80%8F%E9%81%8Ejni-%E8%B7%91cc-for-android-hello_world%E7%AF%87/](http://www.pupuliao.info/2013/05/%E5%9C%A8eclipse%E9%80%8F%E9%81%8Ejni-%E8%B7%91cc-for-android-hello_world%E7%AF%87/)





