---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-21 13:58:53+00:00
slug: '%e7%a0%94%e8%a8%8e%e6%9c%83%e5%bf%83%e5%be%97-paas-meetup-3-msft-azure-ibm-bluemix'
title: '[研討會心得] PaaS Meetup #3 MSFT Azure/ IBM BlueMix'
wordpress_id: 1492
categories:
- 研討會心得
- PaaS
---

**心得:**






  * MSFT Azure 在宣傳資料上面，似乎數據相當的嚇人．之前使用的經驗上還算不錯．



    * PaaS服務其實挺多的，只是支援語言有點少．可能要使用還是以IaaS為主．



  * IBM BlueMix 網路的服務其實很不錯 (hub.jazz.net) 甚至有IOT的SDK，這是比較有競爭力的地方． 不過使用狀況上不算快，可能是台灣的網路都不快，看IBM哪時候把骨幹加強．



    * 有一些免費的quata 其實可以去使用，只是速度堪慮． IOT目前不收費，可能是比較有趣的地方．

<!--more-->



**速記:**






  * **MSFT Azure**



    * 講者: Cacafly  James Jan


    * Cacafly



      * FB 台灣廣告總代理


      * Azure 台灣代理



    * Media Service: 



      * Streaming broadcasting.


      * Support:



        * VOD


        * DRM (PlayReady)


        * Live Streaming


        * 線上轉檔



          * 上傳媒體檔案後，鏈結會根據平台的不同連上不同的streaming. (HLS...…)




      * 參考：



        * [http://msdn.microsoft.com/en-us/library/azure/hh973629.aspx#get_started](http://msdn.microsoft.com/en-us/library/azure/hh973629.aspx#get_started)


        * [http://azure.microsoft.com/en-us/documentation/articles/media-services-set-up-computer/](http://azure.microsoft.com/en-us/documentation/articles/media-services-set-up-computer/)




    * PaaS:



      * Github sync 



        * Once it link, any commit in github will push to Azure directly.



      * Language support:



        * PHP/.NET/nodeJS  直接用挑選的方式


        * 參考:



          * PHP configurare in Azure [http://azure.microsoft.com/zh-tw/documentation/articles/web-sites-php-configure/](http://azure.microsoft.com/zh-tw/documentation/articles/web-sites-php-configure/) 





    * Mobile Services:



      * 可以挑選平台 (iOS/Android/ Windows Store) 然後下載範例程式碼．（類似 [Parse](http://parse.com/))


      * 參考：



        * [http://azure.microsoft.com/zh-tw/documentation/articles/web-sites-php-configure/](http://azure.microsoft.com/zh-tw/documentation/articles/web-sites-php-configure/)


        * iOS [http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/)




    * Machine Learning



      * 有支援R Language ，也有 Hadoop 可以使用．


      * 參考：



        * [http://azure.microsoft.com/en-us/trial/get-started-machine-learning/](http://azure.microsoft.com/en-us/trial/get-started-machine-learning/)





  * **IBM BlueMix**



    * 講者:  Joseph Chang



      * 原本做: [http://www-01.ibm.com/software/tw/rational/](http://www-01.ibm.com/software/tw/rational/)


      * Bluemix 的網址:  [http://www.bluemix.net](http://www.bluemix.net)     


      * Slide:



        * [http://www.slideshare.net/JosephChang8/ibm-blue-mix-introduction](http://www.slideshare.net/JosephChang8/ibm-blue-mix-introduction)



      * Hackpad:



        * [https://paas-tw.hackpad.com/IBM-Bluemix-Introduction-o6WbKTL07n3](https://paas-tw.hackpad.com/IBM-Bluemix-Introduction-o6WbKTL07n3)




    * 基於CloudFoundry 但是IBM 有加強許多的功能．細節可以到[Hackpad](https://paas-tw.hackpad.com/IBM-Bluemix-Introduction-o6WbKTL07n3) 去看



      * IOT: 支援 NoSQL 的SDK在許多IOT device (RPI, ARM…)



        * 參考: [https://developer.ibm.com/iot/](https://developer.ibm.com/iot/)



      * Big Data



        * 參考： [http://www.ng.bluemix.net/docs/#services/AnalyticsWarehouse/index.html#AnalyticsWarehouse](http://www.ng.bluemix.net/docs/#services/AnalyticsWarehouse/index.html%23AnalyticsWarehouse)



      * Mobile Service SDK



        * 參考： [https://www.ng.bluemix.net/docs/#services/mas/index.html#gettingstarted](https://www.ng.bluemix.net/docs/#services/mas/index.html%23gettingstarted)



      * Boilerplates (Web App Server + DB) 



        * 可以快速的建立server 加上資料庫． 第一次建立與瀏覽的時候才會去建置．


        * 參考：[https://www.ng.bluemix.net/docs/#starters/javaDB/index.html#javawebdatabase](https://www.ng.bluemix.net/docs/#starters/javaDB/index.html%23javawebdatabase)




    * 優點：



      * 提供file structure 可以直接去瀏覽PaaS container 的檔案架構．


      * 網路的編輯平台



        * Git migration (Cloud Foundry並沒有)  



          * IBM Git Server not github.  (hub.jazz.net)



        * 可以線上編輯，並且直接在線上去commit與build也可以直接線上deploy





  * 其他參考：



    * Cassandra  (NoSQL for Big Data, used by NetFlix, Twitter, Hulu…)



      * [http://zh.wikipedia.org/wiki/Cassandra](http://zh.wikipedia.org/wiki/Cassandra)


      * [http://www.slideshare.net/jericevans/cassandra-explained](http://www.slideshare.net/jericevans/cassandra-explained)


      * Scala client for Cassandra [https://blog.twitter.com/2012/cassie-scala-client-for-cassandra](https://blog.twitter.com/2012/cassie-scala-client-for-cassandra)




