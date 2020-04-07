---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-07 11:49:28+00:00
slug: iosmapkit-%e8%88%87-google-map-sdk%e7%9a%84%e4%bd%bf%e7%94%a8%e7%ad%86%e8%a8%98
title: '[iOS]MapKit 與 Google Map SDK的使用筆記'
wordpress_id: 1438
categories:
- iOS的程式開發
---

週三又有GTUG的活動，這次是Google Map SDK的講解，於是剛好把iOS 內建的MapKit與Google Map SDK都玩了一次．  
順便記錄一下簡單的心得與可能容易遇到的問題，如下：






  * iOS MapKit



    * 心得：



      * 先不論功能上與GoogleMapSDK的比較，iOS內建的MapKit真的容易使用多了．也不需許多麻煩的東西（等等會提到）



    * 教學可以參考：



      * [http://www.raywenderlich.com/21365/introduction-to-mapkit-in-ios-6-tutorial](http://www.raywenderlich.com/21365/introduction-to-mapkit-in-ios-6-tutorial)


      * [http://www.raywenderlich.com/13160/using-the-google-places-api-with-mapkit](http://www.raywenderlich.com/13160/using-the-google-places-api-with-mapkit)



    * 其他：



      * 比較需要知道的可能就是如何把地址字串直接找到位置而不是使用經緯度．[這篇](http://stackoverflow.com/questions/9606031/ios-mkmapview-place-annotation-by-using-address-instead-of-lat-long)可以參考． 




  * Google Map SDK



    * 心得：



      * 看起來功能似乎很多，但是使用上有許多的地方要注意．



    * 教學可以參考:



      * [https://developers.google.com/maps/documentation/ios/start?hl=zh-TW](https://developers.google.com/maps/documentation/ios/start?hl=zh-TW)



    * 可能遇到問題:



      * Crash when use Google SDK API 



        * Add “-ObjC” in  “Build Setting”->”Other Linker Flag”



      * UIView 與 GMSMapView的使用問題



        * 參考[這篇](http://stackoverflow.com/a/17754975/1316907)，裡面提到的三種方式都很清楚．




    * 其他:



      * 也是可以從字串定點，不過需要透過Google Map API來查然後去讀取JSON回復，這裡有整理好的code [https://gist.github.com/kkdai/7e7cb6d626794829771b](https://gist.github.com/kkdai/7e7cb6d626794829771b)




  * 最後放一篇關於 Google Map SDK與MapKit 更深入研究的一篇文章 [http://www.fastcolabs.com/3006725/open-company/depth-comparison-between-ios-map-frameworks-apple-mapkit-vs-google-maps-sdk](http://www.fastcolabs.com/3006725/open-company/depth-comparison-between-ios-map-frameworks-apple-mapkit-vs-google-maps-sdk)


