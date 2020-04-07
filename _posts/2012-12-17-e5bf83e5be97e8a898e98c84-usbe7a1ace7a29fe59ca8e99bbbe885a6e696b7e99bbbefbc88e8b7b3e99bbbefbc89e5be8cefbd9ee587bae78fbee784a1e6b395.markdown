---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-12-17 17:58:03+00:00
slug: '%e5%bf%83%e5%be%97%e8%a8%98%e9%8c%84-usb%e7%a1%ac%e7%a2%9f%e5%9c%a8%e9%9b%bb%e8%85%a6%e6%96%b7%e9%9b%bb%ef%bc%88%e8%b7%b3%e9%9b%bb%ef%bc%89%e5%be%8c%ef%bd%9e%e5%87%ba%e7%8f%be%e7%84%a1%e6%b3%95'
title: '[心得記錄] USB硬碟在電腦斷電（跳電）後～出現無法讀取而且需要格式化如何救援？'
wordpress_id: 1179
categories:
- 柴米油鹽的東西
---

其實說是心得記錄～很可能只是快速的紀錄一些小技巧跟相關的軟體名稱  
如果你是搜尋過來的千萬注意看以下的一些部分




 




**發生經過：**




使用USB隨身硬碟在把保存在裡面的資料讀取出來，但是因為電腦斷電的原因。  
重新開機後～發現usb隨身硬碟變成無法讀取並且跳出尚未格式化請求你格式化的時候  
這個時候千萬不要輕易的格式化，避免資料無法順利救援出來




 




**救援過程：**






  * 本來找尋一些論壇有建議使用FinalData來讀取，但是因為讀取太久


  * 找尋到[這篇文章](http://www.coolaler.com/archive/index.php/t-59523.html)，裡面有提到SPFDisk的救援方式～來救援分割表的錯誤


  * 關於SPFDisk的使用方式及救援方式，請參考[這篇文章](http://acic1976.pixnet.net/blog/post/33597575-%E4%BD%BF%E7%94%A8-spfdisk-%E4%BE%86%E9%80%B2%E8%A1%8C%E7%A3%81%E7%A2%9F%E6%95%91%E6%8F%B4%EF%BC%81)


  * 選擇非破壞性救援後～發現資料還是無法正常讀取～開始尋找一些軟體的幫助


  * FinalData2.0 -->  無法修復硬碟～看起來有實體壞軌的狀態（心理準備～有檔案要犧牲了）


  * R-Studio 



    * 5.0 -->   看起來遇到壞軌會有crash的狀態，


    * 6.0 -->   果然正確的救援初所有的資料～而且300G的硬碟硬生生找出500G的資料（雖然很多壞掉了）





果然使用R-Studio 是相當令人滿意～這裡也附上該公司網頁




[http://www.r-studio.com/](http://www.r-studio.com/)
