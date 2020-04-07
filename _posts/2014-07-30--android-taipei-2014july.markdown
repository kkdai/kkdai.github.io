---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-30 16:39:31+00:00
slug: '%e7%a0%94%e8%a8%8e%e6%9c%83%e8%a8%8a%e6%81%af-android-taipei-2014july'
title: '[研討會訊息] Android Taipei - 2014/July'
wordpress_id: 1469
categories:
- Android安卓學習心得
- 研討會心得
---

**心得：**




Android Taipei 有好幾次的活動不是跟GTUG對衝，就是剛好遇到颱風．這次剛好有機會可以參加（其實是[GTUG](http://gdg-taipei.kktix.cc/events/javafx4android) [JavaFX](http://www.slideshare.net/steveonjava/presentations)不太懂 XD)  
所以今天就去了，恰巧也有機會揪了以前的同事一起去(Nick，Veret)．  
以下簡單的記錄一下三個講者的內容跟心得：






  * MonkeyRun:



    * 這個東西主要是透過MonkeyRecorder 來錄下自動測試的腳本，搭配本身有的一些進階功能可以拍下截圖並且寄出錯誤資訊．講者有增近一些功能後OpenSource出來，挺值得一看．


    * 會後有找講者討論了一下這個東西能導入的狀況與真正幫助到測試的範圍：



      * 新版本出來以後，分成兩個部分．一部份還是QA來跑，另外交給自動測試．隨著QA測試錄下的腳本增加，需要真人的部分可以越來越少．


      * 自動測試主要是跑一些比較簡單還有透過截圖的比較，測試流程的減少看來有20%左右（透過了解的估計）




  * Chrome Cast (或者稱為 Google Cast)



    * 主要講解整體架構，基本上就是App(Sender)透過通訊協定讓receiver 去播放網路上的資料（是網路上的而非手機上的）


    * 也稍微有講解了一下可能遇到的問題，下面都有提到．


    * 會後有跑去跟講者討論一下是否可能直接手機透過streaming 到server上後再cast 到receiver，看來是可以的只是造成的latency看來是很難突破．(IP-CAM?)



  * Google IO 2014 分享



    * 主要是講者分享一下GOOGLE IO 2014在當地參加的心得與內容，由於我有到臺北Google辦公室看過直播，所以還好．


    * 之後講者有把這次有趣的玩具 [Cardboard](https://www.google.com/events/io/schedule/session/603fe228-89c5-e311-b297-00155d5066d7)讓大家體驗一下，其實讓我相當驚訝的是，本來以為會像3D眼鏡，其實不然～是真的很像VR虛擬螢幕在前面．透過G-Sensor可以調整．


    * 它提供的SDK就是可以切割兩個螢幕外，還用突透鏡羽還原的技巧來讓立體成像更為清楚．彷彿眼前有個大螢幕在前面，還可以上下擺動看到不同的東西．





**速記:** 






  * GogoMonkeyRun [MonkeyRunner](http://developer.android.com/tools/help/monkeyrunner_concepts.html)  ([gogolook](http://whoscall.com/about-us/))



    * Auto testing



      * Android Instrumentation test


      * uiautomator


      * monkey/monkey runner



        * monkey runner — using python code to execute auto testing 


        * Using python to click specific point. (x,y)




    * Monkey runner 不足



      * Q:  How to determine x, y.



        * A:  using “monkey recorder”, project cellphone to PC.




    * 常出現問題:



      * 不能截圖


      * 不能取消動作


      * 不能back


      * 畫面被壓縮


      * 拖曳無法知道起始點與終點


      * 無法旋轉



    *  gogolook 強化: github-> ([gogomonkeyrun](https://github.com/Gogolook-Inc/GogoMonkeyRun))



      * 特點:



        * 可選擇安裝apk


        * 可以截圖


        * 可以寄信給開發者


        * 可以按back


        * *修改App內容


        * *遠端控制



      * 使用:



        * 先用人工點一次，結束後可以產生相關動作的python code


        * 如果要跑不同的機型，需要執行多次的monkey runner 


        * 跑的截圖可以做比較，來發現說是否有錯誤和異常．


        * 可以用來catch exception  然後寄信給開發者




    *  辛酸史:



      * Monkey recorder 跑完馬上跑monkey runner 是會出現錯誤，device 被decoder 佔走



        * 解決方法：  **shell stop**



      *  Windows 到Mac 容易出現問題


      * Google 從2012年後就沒有更新



    * Feature works:



      * 產生更豐富的資訊


      * Jenkins 結合 (other [Android Emulator](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin))


      * 手機端看結果



    * 參考:



      * [http://fecbob.pixnet.net/blog/post/35999435-android%E8%87%AA%E5%8B%95%E6%B8%AC%E8%A9%A6%E4%B9%8Bmonkeyrunner%E5%B7%A5%E5%85%B7](http://fecbob.pixnet.net/blog/post/35999435-android%E8%87%AA%E5%8B%95%E6%B8%AC%E8%A9%A6%E4%B9%8Bmonkeyrunner%E5%B7%A5%E5%85%B7)




  * Chromecast SDK （google cast) (kkbox: Ascii)



    * 基本概念:



      * Sender:  手機


      * Receiver  電視



    * 使用方法:



      * 新增一個button 可以share 到電視 (MediaRoutingButton)


      * 每個15秒換待機圖


      * sender will send application ID, receiver will determine to open which application (web + java script)



    * 注意事項:



      * Chrome cast debug console: port 9222 (web debug console)



        * 出現404  -> 需要到設定把 [檢查更新把序號送到Chrome cast) 打勾




    * About chrome cast



      * Sender app



        * Android


        * iOS


        * Chrome Sender



      * Receiver app



        * default media player


        * style media player 


        * custom player 



          * 任何都可以，動態歌詞，界面





    * Register application developer account



      * each chrome cast developer need $5.


      * Default Media Receiver



        *  (不需要花$5)


        * 不需要架設https




    * 開發中遇到的問題：



      * Resolution (亂跳)



        * Style receiver 會先用1080p


        * custom receiver 會先跳 720p


        * 解法 



          * **這是google bug, 如果硬用720p 等google 修掉會出現空白**




      * MediaRouterButton 一直grey out



        * 解法：



          * 不同network


          * 先使用 “CC1AD845” (default app ID 去測試)





    * Chrome Cast SDK



      * Media Player



        * Sender:



          * RemoteMEdiaPlayer.load



        * Receiver:



          * call onLoad




      * SendMessaage



        * 注意:



          * Receiver 需要 'urn:x-cast: com.xxx.xxx’




      * Github demo: code [https://github.com/AsciiHuang/CastPractice](https://github.com/AsciiHuang/CastPractice)



    * DRM: 



      * MSFT playready


      * Google DRM


      * kkbox using KKbox-DRM



    * Chrome Cast capability:



      * [Streaming type](https://developers.google.com/cast/docs/media):



        * Streaming :  MP4, WebM


        * Adaptive Streaming : HLS


        * [https://developers.google.com/cast/docs/media](https://developers.google.com/cast/docs/media)




    * 參考:



      * [https://developers.google.com/cast/docs/developers](https://developers.google.com/cast/docs/ios_sender)


      * [https://developers.google.com/cast/docs/ios_sender](https://developers.google.com/cast/docs/ios_sender)


      * [http://chrome.blogspot.tw/2014/02/chromecast-is-now-open-to-developers.html](http://chrome.blogspot.tw/2014/02/chromecast-is-now-open-to-developers.html)


      * [https://github.com/AsciiHuang/CastPractice](https://github.com/AsciiHuang/CastPractice)




  * Google IO 經驗



    * Android Wear (各一個)



      * Moto 360


      * Samesung / LG



    * Android Auto


    * Android TV


    * Android L



      * Animation:



        * Rendering 不再拘束在main thread. —> UI Toolkit thread



      * Shader



        * 4 Layers




    * Cardboard



      * 有提供SDK 去處理分割畫面與凸透鏡的效果


      * 可以做到VR




