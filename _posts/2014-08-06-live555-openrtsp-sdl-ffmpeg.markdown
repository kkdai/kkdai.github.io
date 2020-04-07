---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-06 02:59:41+00:00
slug: live555openrtspsdlffmpeg-%e5%88%a9%e7%94%a8ffmpeg-%e8%88%87sdl-%e9%81%94%e6%88%90-streaming-%e7%ad%86%e8%a8%982
title: '[live555][OpenRTSP][SDL][ffmpeg] 利用ffmpeg 與SDL 達成 streaming 筆記(2)'
wordpress_id: 1477
categories:
- ffmpeg
- RTSP
- VC++程式設計
---

最近有一些值得記錄的部分，隨手寫一下:






  * **SDL_CloseAudio 會發生一些問題 (deadlock)，如果Server 已經斷線了．**



    * **發生狀況：**



      * 當RTSP Server斷線或是網路中斷的時候，嘗試著去Close SDL 會產生deadlock 在 SDL_CloseAudio


      * 如果封包持續進來的狀況下，不會有任何問題．



    * **主要原因：**



      * 根據SDL source code， RunAudio 裡面會去 OpenMutex讀取資料．這個狀況下，如果Server斷線（或是網路斷線) 會造成一直在等封包的結束．必須等到每個封包做完後會SDL_Delay特定時間，這時候就可以順利關閉設備．



    * **解決方式：**



      * 為了解決這種deadlock的狀況，要試著去關閉設備的時候，先送一個空的封包進去．讓SDL跑到等待下一個封包的進入才能順利關閉．



    * **參考:**



      * [http://stackoverflow.com/questions/24926019/try-to-close-sdl-closeaudio-has-deadlock-when-rtsp-server-is-down/24926020#24926020](http://stackoverflow.com/questions/24926019/try-to-close-sdl-closeaudio-has-deadlock-when-rtsp-server-is-down/24926020#24926020)




  * **SDL_DestroyWindow 有threading 的問題，SDL_CreateWindow 必須要跟SDL_DestroyWindow 在同一個thread**



    * **發生狀況：**



      * 使用SDL_DestroyWindow 會發生no response(hang)，如果SDL_DestroyWindow與SDL_DestroyWindow在不同的thread．



    * **解決方式：**



      * 本來都以為是因為必須在UI thread，但是由於我自己架設的環境RTSP 是在另外一個thread去執行，以致於不會卡住所有UI response．


      * 後來發現用另外一個thread來處理RTSP，也是可以讓SDL來render畫面與聲音的輸出．但是在SDL_DestroyWindow的時候就會卡住．


      * 後來調整後，就發現可以正常的SDL_DestroyWindow．



    * 參考：



      * [http://forums.libsdl.org/viewtopic.php?p=40752&sid=245abd9df9be7b7ecf6dc59d7388dc66](http://forums.libsdl.org/viewtopic.php?p=40752&sid=245abd9df9be7b7ecf6dc59d7388dc66) 



        * 這個討論串主要是談到iOS不過似乎Windows也是一樣的．







SDL_DestroyWindow mongo kahana.mongohq.com:10042/MongoTest1 -u <user> -p<password>
