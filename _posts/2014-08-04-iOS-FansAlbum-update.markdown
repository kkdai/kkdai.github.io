---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-04 10:01:25+00:00
slug: ios%e7%b2%89%e7%b5%b2%e7%9b%b8%e7%b0%bf%e6%9b%b4%e6%96%b0v1-2-%e4%b8%bb%e8%a6%81%e6%98%af%e4%bf%ae%e5%be%a9%e4%b8%80%e4%ba%9b%e6%af%94%e8%bc%83%e5%a4%a7%e7%9a%84%e5%95%8f%e9%a1%8c%e9%82%84
title: '[iOS][粉絲相簿更新v1.2] 主要是修復一些比較大的問題還有視覺icon調整'
wordpress_id: 1475
categories:
- App-粉絲相簿
---

![](https://raw.githubusercontent.com/kkdai/iOS-APP-FBAlbums/master/img/1.1/icon.png)




**前言：**




在07/23更新了[粉絲相簿](https://itunes.apple.com/tw/app/fen-si-xiang-bu/id839324997?l=zh&mt=8)到v1.2版本，主要是修復不論是視覺界面的問題，還有操作上的一些問題．

<!--more-->


**主要修復:**






  * 美化icon解析度，修復視覺問題


  * 修復使用者端資料庫會產生錯誤讀取的問題


  * 修復許多操作上的問題




**雜言：**




這次更新的上架速度比我想像中的快了很多，大概一個禮拜就可以正式上架．果然更新跟第一次上架有天壤之別．  
此外～自己來寫app真的很難做完全的測試．每次都是上架後，朋友才會來跟我講哪裡有問題，哪裡有錯誤．  
這樣一直更新都讓自己覺得不好意思  XD




 




最近有人問我一些問題，在這裡寫一下幾個FAQ






  * Q： 這個APP寫了多久？



    * Ａ：這個ＡＰＰ總共前前後後寫了一年多．從我買Macbook Air到現在 ．前後貫穿了從我完全不會寫iOS到現在（也沒多懂@_@）



  * Q：這個App有用到哪些技術呢？



    * 這個問題比較複雜一點，裡面主要有用到幾個ＳＤＫ:


    * [Facebook SDK](https://developers.facebook.com/docs/ios)



      * 主要就是讀取粉絲相簿與資料，其中也包括了 Single-Sign-On



    * [Parse SDK](https://parse.com/)



      * 就是網路伺服器與資料庫管理的部分使用Parse，一個簡單使用並且在某些流量下是免費的PaaS．


      * 可以讓我存取一些資料庫，還可以發送push notification．



    * [FGallery](https://github.com/gdavis/FGallery-iPhone)



      * 主要負責的就是各位看到的相簿瀏覽的部分，我知道裡面有許多的問題．比如說：不能批次讀取，滑動會上下，不能暫存等等．


      * 這些部分的加強就會擺在之後，當然也是我更熟悉後．




  * Q：App的資料哪裡來？



    * A：主要都是網友分享的，所以也希望又更多網友能夠分享你們喜歡的粉絲頁面給大家，讓這個更好用．





 




 




 
