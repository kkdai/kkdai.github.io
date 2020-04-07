---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-01-24 01:10:33+00:00
slug: android%e9%97%9c%e6%96%bc%e6%89%8b%e6%a9%9f-galaxy-nexus-root-%e5%9c%a8mac-os%e4%b8%8aroot
title: '[Android]關於手機 Galaxy Nexus Root 在Mac OS上Root'
wordpress_id: 1298
categories:
- Android安卓學習心得
---

由於要練習FireFox OS跟 Ubuntu OS於是還是做了點小投資買了一檯二手的Android Galaxy Nexus來刷  
(由於在FFOS 與 Ubuntu OS 的device list找到比較便宜與容易購買的就是這個)




**使用心得:**  
由於我自己習慣使用iPhone跟設計iPhone的App，其實很不習慣Android的“上一頁”實體按鍵．不過多使用幾次之後其實也還好了． 也發現Android沒有iOS的一些方便的功能如下:






  * **Tab bar:** 也就是下方的Tab按鍵，這是Android App RD要花心思去搞定的．


  * **Storyboard:** Android 使用Activity設計概念並且用Intent來溝通，但是沒有StoryBoard的階層觀念．




其他細節等到我其它文章再寫..




 




**關於Root 手機**






  * 打開Development mode. [開發人員選項]



    * 打開[設定] -> [關於手機] -> 選到[版本號碼] ( 大概點個七下) 啓動  (這是什麼怪東西？)



  * 進入 [設定]->[開發人員選項]-> [USB 偵錯]  啓動


  * 要Root 前，必須要先解開Boot loader lock的限制才能夠Root



    * 下載 [http://downloadandroidfiles.org/download-unlock-bootloader-gsm-verizon-sprint-zip/](http://downloadandroidfiles.org/download-unlock-bootloader-gsm-verizon-sprint-zip/)


    * 進入 [FASTBOOT Mode] 



      * 先關機


      * 左手按下兩個音量鍵 （兩個都要按)不要放


      * 再按下 電源鍵



    * 這時候手機會進入FastBoot Mode (也就是有機器人躺在那邊)


    * 執行剛剛下載的檔案


    * 這時候會進入警告標語


    * 利用 音量鍵選擇 ，電源鍵確認



  * 解開後，手機的OS會重灌


  * 一樣的步驟


    * 進入 [設定]->[開發人員選項]-> [USB 偵錯]  啓動


    * 進入 [FASTBOOT Mode] 





  * 下載 [Root kit] [http://downloadandroidfiles.org/download-updated-galaxynexusroot-gsm-verizon-sprint-zip/](http://downloadandroidfiles.org/download-updated-galaxynexusroot-gsm-verizon-sprint-zip/)



    * 執行先解開 [Boot Loader Lock]


    * 開機 -> 設定 -> USB 偵錯


    * 按下enter繼續跑


    * 選擇(1)  GSM ClockworkMod Recovery (Version 6.0.2.3)


    * 利用 [音量鍵] 改到 [Recovery mode] -> 電源鍵執行


    * 選擇 install from SD card 


    * 安裝位於 目錄 0/UPDATE-SuperSU-v1.45.zip


    * 安裝跑完最後選擇No


    * 重開機



  * 開機完 看到應用程式裡面有 [SuperSU] 代表你Root 成功．




**參考資料：**






  * [http://dottech.org/87439/how-to-unlock-usb-debugging-mode-on-android-4-2-jelly-bean-and-higher-guide/](http://dottech.org/87439/how-to-unlock-usb-debugging-mode-on-android-4-2-jelly-bean-and-higher-guide/)


  * [http://www.androidrootz.com/2012/12/galaxy-nexus-one-click-mac-toolkit.html](http://www.androidrootz.com/2012/12/galaxy-nexus-one-click-mac-toolkit.html)


  * [http://forum.xda-developers.com/showthread.php?t=1790399](http://forum.xda-developers.com/showthread.php?t=1790399)


