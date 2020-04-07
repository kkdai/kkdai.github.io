---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-13 12:22:50+00:00
slug: moocjavaprogramming-cloud-service-%e9%97%9c%e6%96%bcspring-boot-%e7%9a%84%e5%9f%ba%e6%9c%ac%e5%ad%b8%e7%bf%92-%e5%ae%8c%e6%88%90%e7%ac%ac%e4%b8%80%e6%ac%a1%e4%bd%9c%e6%a5%ad%e5%bf%83%e5%be%97
title: '[MOOC][Java][Programming Cloud Service] 關於Spring Boot 的基本學習 -完成第一次作業心得與筆記'
wordpress_id: 1487
categories:
- 網路課程筆記
tags: ["spring", "java"]

---

**前言：**




這些學習主要都是針對[Mooc 上面的課程](http://www.coursera.org/course/mobilecloud)(Programming Cloud Service)所學到的一些部分．  
雖然說同時在學習Golang跟Java 是有一些混亂的，能夠學習一下關於Java上面架設REST Server的方法有是挺有趣的．




**第一個作業大綱與心得： **




這個課程的第一份作業其實相當的有趣，就是實作一個REST 的server可以查詢video info 還有就是能夠上傳video．  
雖然相當有趣，但是其實讓我相當的苦惱．就是整個Controller的parameter到底該怎麼設計才能讓Retrofit  能夠正確的讀取與呼叫到．  
其實如果Java或是Spring有點熟悉的人應該可以快速地完成第一次的作業．我卻也繳了不少時間當學費來好好學習整個架構與溝通的方法．




[**Spring Boot:**](http://spring.io/guides/gs/spring-boot/)



<!--more-->


  * **簡介：**



    * [Spring Boot](http://spring.io/guides/gs/spring-boot/) 可以快速的幫助你建立一個Java Based 的俱有REST 的Web Application．



  * **使用上的筆記:**



    * 比較需要管理的只有兩個部分，一個是Application.java 另外一個是Controller.java ．其中檔名可以改，但是需要有annotation 來表明清楚哪個是application 哪個是 controller．


    * 其中關於Controller裡面，重點是需要有 annotation 註名是 controller



      * @Controller



    * 相較於Java Serverlet 對於 Web Request 的處理上，Spring Boot 相對的簡單多了．不需要繼承 HttpServlet 然後去 overwrite doGet 跟 doPost，只需要在前端去註解出這個function 要對應到哪個http request．



      * @RequestMapping(value=“videos", method=RequestMethod.POST)


      * 這個就是代表著 ，只要從http XXXXvideos的 POST request 都會轉到這個地方．



    * 對於去處理Http Request的部分，最麻煩了不外乎是處理 輸入與輸出的參數，而Spring Boot 可以很漂亮的做出這件事情．



      * @RequestBody/@ResponseBody


      * 這個會自動地把參數轉成你所要求的，不論是把Boolean轉成@ResponseBody(JSON的形態） 或是把@RequestBody轉成你所要求的變數形態．



    * 更改運作的port (change connection port) [官方提出兩個方法，不過我是第二個才成功](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-change-the-http-port)．



      * 新增 application.properties 在 src/main/resources/  並且加上 server.port = 9000


      * 在 Eclipse 的properties -> Run/Debug Settings -> Application -> Environment -> Add [SERVER_PORT] = 9000



    * 同時需要debug - Server 與 Client (test)



      * 先跑 Server - debug as Java Application (設定好break point)


      * 再接者跑test - debug as JUnit test


      * 在同時做 server  與 client debug 的時候，常常會有一個動作卡在 client 之後 server就沒回應．或是專心的看server的code，client 就停住了． 我會繼續研究看看．



    * 對於 /path/{id} 這一類型的RequestMapping 需要使用以下，不然會找不到:  


    
    <span style="box-sizing:border-box;color:#006666;" class="lit">@RequestMapping</span><span style="box-sizing:border-box;color:#666600;" class="pun">(</span><span style="box-sizing:border-box;color:#000000;" class="pln">value</span><span style="box-sizing:border-box;color:#666600;" class="pun">=</span><span style="box-sizing:border-box;color:#008800;" class="str">"/departments/{departmentId}"</span><span style="box-sizing:border-box;color:#666600;" class="pun">)<br></br></span><span style="box-sizing:border-box;color:#000088;" class="kwd">public </span><span style="box-sizing:border-box;color:#660066;" class="typ">String</span><span style="box-sizing:border-box;color:#000000;" class="pln"> findDepatmentAlternative</span><span style="box-sizing:border-box;color:#666600;" class="pun">(</span><span style="box-sizing:border-box;color:#006666;" class="lit">@PathVariable</span><span style="box-sizing:border-box;color:#666600;" class="pun">(</span><span style="box-sizing:border-box;color:#008800;" class="str">"departmentId"</span><span style="box-sizing:border-box;color:#666600;" class="pun">)</span><span style="box-sizing:border-box;color:#660066;" class="typ">String</span><span style="box-sizing:border-box;color:#000000;" class="pln"> someDepartmentId</span><span style="box-sizing:border-box;color:#666600;" class="pun">)<br></br>{… </span><span style="box-sizing:border-box;color:#666600;" class="pun">}</span>





    * **對於multipart 的敘述，一開始不是很了解會以為是POST的必須的API．回去看了關於[Retrofit](http://square.github.io/retrofit/) 的敘述才知道Multipart只針對檔案的上傳**



      * **他在controller裡面對應的API是 @RequestParam(”data”) MultipartFile uploadFiles…**



    * 其實在實作controller的時候，是不需要參考Retrofit 的API，那個API只是幫助你的test client 知道該怎麼跟server溝通．


    * 很多參數在Controller是可以基本帶入的:



      * HttpServletResponse 跟 HttpServletRequest 可以加入在parameter內然後看看有沒有呼叫到．




  * 參考：



    * Basic guide in Spring Boot



      * [http://spring.io/guides/gs/spring-boot/](http://spring.io/guides/gs/spring-boot/)


      * [http://docs.spring.io/spring-boot/docs/1.0.1.RELEASE/reference/html/index.html](http://docs.spring.io/spring-boot/docs/1.0.1.RELEASE/reference/html/index.html)


      * [http://spring.io/guides/gs/serving-web-content/](http://spring.io/guides/gs/serving-web-content/)



    * How to change port in Spring.



      * [http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-change-the-http-port](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-change-the-http-port)



    * Spring PathVariable



      * [http://www.byteslounge.com/tutorials/spring-mvc-requestmapping-example](http://www.byteslounge.com/tutorials/spring-mvc-requestmapping-example)


      * [http://www.baeldung.com/spring-requestmapping](http://www.baeldung.com/spring-requestmapping)



    * About Multipart



      * [http://square.github.io/retrofit/](http://square.github.io/retrofit/)


      * [http://stackoverflow.com/questions/24985832/405-method-not-allowed-with-spring](http://stackoverflow.com/questions/24985832/405-method-not-allowed-with-spring)




