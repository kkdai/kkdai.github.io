---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-09 13:59:17+00:00
slug: '%e7%a0%94%e8%a8%8e%e6%9c%83%e5%bf%83%e5%be%97-gdg-taipei-9-furnish-you-app-with-maps'
title: '[研討會心得] GDG Taipei #9 - Furnish you app with Maps'
wordpress_id: 1445
categories:
- Android安卓學習心得
- 研討會心得
- iOS的程式開發
---




**心得:**




這次來聽Google Map [GDE](https://developers.google.com/experts/) [Homing Tam](https://plus.google.com/u/0/+HomingTam/posts)講解關於Google Map的簡介，由於時間的因素主要都是在講解Google Map相關延生的產品．不過內容上相當的有趣．也有新舊版本上的使用差異，從眾多的Google Map所衍生出來的產品也更容易了解Google Map API與Google Map如何使用的想法． 會後其實也花了一些時間好好地觀看了一下Google IO帶回來的三星Android Wear實體機．也知道了台灣除了[Colin Su](https://plus.google.com/106984014018383590693)之外又多了一個[GDE ](https://developers.google.com/experts/) [David Chen](https://plus.google.com/+LucemiaChen/posts) (不過台灣現在都是Google Cloud 的Expert)．



<!--more-->
  





**速記：**  







  * Google Maps Feature update


  * introducing the Google Maps product families



    * [https://www.google.com/maps/about/partners/businessview/](https://www.google.com/maps/about/partners/businessview/) Google business view (business photo)


    * [http://maps.google.com/gallery/](http://maps.google.com/gallery/) Google map with data in gallery


    * [https://www.google.com/mars/](https://www.google.com/mars/) Google Mars Map


    * [https://www.google.com/moon/](https://www.google.com/moon/) Google Moon Map


    * [https://www.google.com/sky/](https://www.google.com/sky/) Google Sky View


    * [https://www.google.com.tw/mapmaker](https://www.google.com.tw/mapmaker) Google Map Marker (自定修改地圖)


    * [https://mapsengine.google.com/map/](https://mapsengine.google.com/map/) Google Map Engine



      * 有分Lite，Pro與Mobile版本．



    * [http://www.google.com/enterprise/mapsearth/products/coordinate.html](http://www.google.com/enterprise/mapsearth/products/coordinate.html)  Google Map Coordinate 



      * 可以追蹤人用的，安裝之後手機GPS會強制打開．



    * Google Earth



      * [安全台灣 ](https://www.safetaiwan.tw/) 利用Google Earth Enterprise 自己建立local Google Map Client



    * [https://support.google.com/fusiontables/answer/2571232](https://support.google.com/fusiontables/answer/2571232) Google Fusion Table



      * 地理資訊資料庫，不過有漸漸被淘汰的趨勢




  * indoor map != indoor street view



    * yellow point -> business view



      * certificated agency to take photo and upload to google map.



    * indoor map is a flat map indoor map



      * User upload.



    * indoor street view is using street car to take photo



      * Provide by Google


      * 範例: [雪山隧道人行步道 ](http://goo.gl/maps/2aPSd)




  * 舊版地圖與新版地圖的差異



    * 舊版地圖可以分享 ([參考](http://goo.gl/maps/2aPSd)) 



      * 原因是因為新版地圖的網址是不固定的，所以無法分享．也有IFrame的整合．




  * Google Map API



    * Currently it use v3.13


    * v2 already out of date since 2013/09


    * Why use version 3


    * [差異 (Google Map vs Google Map API for business)](https://developers.google.com/maps/licensing?hl=zh-tw)



      * 25000 PV per day


      * two div treat as two request.






問題：






  * Google Map 愚人節活動 - PokeMon 有打算開放API嗎？



    * 8bit Map 不可能，但是PokeMon 是可以自己做



  * Google Map有考慮加高度嗎?



    * 其實資料有在裡面的， 參考 Google Elevation API



  * Google Map SDK 為何地址查詢經緯度還需要使用Google Map API?



    * 這是故意的，因為Google 地理資訊是希望綁在 Google Map API 裡面



