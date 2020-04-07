---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-02 08:03:23+00:00
slug: wdk-note-about-propitiatory-driver-develop-and-install
title: '[WDK] Note about propitiatory driver develop and install'
wordpress_id: 1432
categories:
- C++溫故知新
- 學習文件
---



  * WDK 



    * building



      * build -cefw


      * build -cgz




  * Driver launch 



    * Service



      * Using SC (Service Control)



    * Device driver



      * Using *.Inf to install it




  * Disable digital signing check ( forever)



    * If failed need go to BIOS setting to disable Security Boot



      * in Boot-> Authentication-> Security Boot



    * **"bcdedit -set test signing ON"**



  * Disable digital signing for once



    * Win8 -> 變更電腦設定 -> 還原 -> 進階開機



  * Install driver step



    * [裝置管理員] -> node 0 -> [動作] -> [新增傳統硬體] -> 選擇 inf


    * remember  inf file need  exist with x86 driver


    * x64 need another place according to your inf setting.


    * Still occur error because the SYS still need to signed 



  *  Install Digital signature and sign your SYS file (x64 only is enough if you using 


  * Note, there might be an error if the category file error



    * Error:



      * 裝置管理員 -> Digital signature still failed event you already signed it.



    * Solution:



      * Using “infract”  refer [here](http://msdn.microsoft.com/en-us/library/windows/hardware/ff547089(v=vs.85).aspx)



        * Note: Need use WDK 8.1 if you want to use Win8.1 OS identifier


        * **“inf2cat /driver:INF_ADDRESS /os:6_3_X86,6_3_X64"**



      * It will find digital signature in 裝置管理員


      * Must use WDK for Win8.


      * Don’t forget to use digital signature to  sign CAT file before you try to install INF.




  * Install INF driver via 裝置管理員



    * Error: 



      * 無法驗證數位簽章



    * Solution:



      * Testsigning need to change from off to On.


      * Reboot again and check 裝置管理員






 
