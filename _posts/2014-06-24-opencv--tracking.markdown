---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-06-24 01:54:15+00:00
slug: opencv-%e7%9b%b8%e6%a9%9f%e5%a2%9e%e5%8a%a0%e7%89%a9%e4%bb%b6%e8%bf%bd%e8%b9%a4object-tracking%e7%9a%84%e9%83%a8%e5%88%86
title: '[OpenCV] 相機增加物件追蹤(Object Tracking)的部分'
wordpress_id: 1423
categories:
- opencv
---

OpenCV是一個相當好用的影像處理SDK，除了可以快速開發相機測試程式之外，也有許多功能可以增加．




最近看到一般數位相機有類似的功能，於是去找了一下．以下是[示意影片](https://www.youtube.com/watch?v=jrfI5r77Zlo)．完整[網址](http://bellcode.wordpress.com/2012/07/19/object-tracking-with-opencvs-templatematching/)在這裡，不過他有用到[OpenFramework](http://www.openframeworks.cc/)去操控OpenCV個人覺得不好使用．比較推薦直接使用OpenCV其實程式會很小．




![NewImage](http://www.evanlin.com/blog/wp-content/uploads/2014/06/NewImage.png)


<!--more-->

 




 研究了一下，發現其實效果不錯．於是整理出一個簡單的 VS2013 Console 的測試程式  
(本來有想用 Python 後來發現需要太多的原件而且很難debug，所以先弄Windows版本) 




[https://github.com/kkdai/OpenCVConsole](https://github.com/kkdai/OpenCVConsole)




這個測試程式主要可以讓我測試一些功能，他目前支援以下一些功能:






  * 起始你的Camera 並且使用最預設的解析度


  * 支援旋轉，他的熱鍵如下:



    * t/T: 90度旋轉


    * f/F: 180度旋轉


    * r/R: 270度旋轉


    * n/N: 回復原狀



  * 灰階化，方便之後做進階的影像處理（熱鍵 g/G)


  * 物件追蹤，目前測試發現辨識率不高，並且灰色階也沒有差異．使用方式如下:



    * 利用你的滑鼠去選取你要追蹤的部分


    * 他會複製起來～並且把你要追蹤的部分用匡線標起來





狀況大概是以下狀態:




![OpenCVOT](http://www.evanlin.com/blog/wp-content/uploads/2014/06/OpenCVOT.jpg)
