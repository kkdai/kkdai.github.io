---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-28 07:57:16+00:00
slug: moocjavaprogramming-cloud-service-%e7%ad%86%e8%a8%98%e8%88%87%e7%ac%ac%e4%ba%8c%e6%ac%a1%e4%bd%9c%e6%a5%ad
title: '[MOOC][Java][Programming Cloud Service] 筆記與第二次作業'
wordpress_id: 1496
categories:
- 網路課程筆記
tags: ["java", "spring"]

---

**前言:**


這些學習主要都是針對[Mooc 上面的課程(Programming Cloud Service)](https://www.coursera.org/course/mobilecloud)所學到的一些部分．
<!--more-->



** 心得：**  
第二個作業不確定是不是最後的作業，但是卻相當的有趣．主要是透過架設OAuth2 的Server 然後去搭配測試程式的Client來做Video是否有喜歡 (like)的操作．  
裡面幾個邏輯都算是很簡單，比如說Video不能被同一個使用者喜歡兩次． 喜歡的Video是必須存在的....  
  
主要的難度，還是在跟Https 與 OAuth2的部分，不過由於我有習慣從第一個範例開始就寫同一個程式然後慢慢去改～慢慢學習裡面的幾個比較奧妙的地方．  
所以這次的作業反而相當快就寫完了．  
  
很值得修的課程，對於 HTTPS， OAUTH都有相當清楚地介紹，想要了解透過Exclipse去架設Java Server (不論是 Jetty 或是 Tomcat）這都是好選擇．  
當然如果你對於Eclipse上面Gradle不太熟，這個也可以告訴你怎麼學習啊～～因為我也卡了很久．主要就是每次加了新component 之後都忘記要 Gradle->Refresh．  
  
最後提一下學這堂課的動機吧，第一個部分是希望好好把Cloud Programming 有系統地學一下，雖然說懂 JSON，REST跟 OAuth．但是詳細的原理跟真正在Java (畢竟JSON是Java出來的)上要如何實現才能真正的了解；另外一個原因就是因為我一直以來對於Java與使用Eclipse有一種排斥感（雖然我修過Scala)，但是我希望透過這個學習會讓我更熟悉這些部分．




我學習的[Github 在這裏](https://github.com/kkdai/Java-Cloud-Learn)




  




**課堂筆記:**






  * @ComponentScan 會搜尋指定範圍的原件，是會重啟Spring．除了一些PaaS可能會踢掉之外（GAE?)，一般而言並不會造成任何效能上的影響．


  * @Autowired 對應著Application.java 的一個@Bean/@Component/@Service/@Repository，可以讓他根據不同狀況來改變injection 不同的class． 如果沒有@Bean在Application 會產生exception．



    * 使用上可以類似C++的繼承方式，使用 @Autowired 在一個parent class上．然後利用@Bean 或其他的方式來mapping 到你需要的child class來改變或是更換你需要的物件．

  * Java Persistence API @Entity @Respository 需要做以下的事情
    * 需要在Grandle 先加上以下的資訊



      * compile("org.springframework.boot:spring-boot-starter-data-jpa:${springBootVersion}”)       


      * compile("jdbc:jdbc:2.0”)    


      * compile("com.h2database:h2”)    



    * 如果要連接一些資料庫，也需要在application.properties裡面設定好



      * spring.datasource.url=jdbc:mysql://localhost/test


      * spring.datasource.username=dbuser


      * spring.datasource.password=dbpass


      * spring.datasource.driverClassName=com.mysql.jdbc.Driver




  * About Data REST



    * 要把整個Spring 從controller 改成由這個改變)



      * Application 繼承RepositoryRestMvcConfiguration


      * Gradle 要加入compile("org.springframework.data:spring-data-rest-webmvc”)


      * 整個Controller 不再需要，直接移除後開始使用 VideoRepository


      * 修改 VideoRepository 傳回與接收直，注意以下事項： （參考[另外一個改變](https://github.com/kkdai/Java-Cloud-Learn/commit/09fb8b3b557b5958ce8a8e0a461c15da4a4b3096))



        * findByXXX 必須回傳 Collection 不然就會錯誤<


        * addVideo 不回傳




    * 重要的是必須要 ObjectMapper弄好～不然傳回[HATEOS](http://en.wikipedia.org/wiki/HATEOAS)就沒辦法直接使用/li>



  * Spring Security 增加一些安全性的保護（登入機制）並且利用加密的cookie來辨別身份，並且可以根據使用者權限來決定可以執行的REST指令或是要求．



    * 要加上https的支援，必須要做以下的修改:



      * 加入mainresourcesprivatekeystone (如何產生看[這裡](http://tomcat.apache.org/tomcat-7.0-doc/ssl-howto.html))


      * 要把Jetty 改成用tomcat



        * compile("org.springframework.boot:spring-boot-starter-web:${springBootVersion}”)    


        * compile("org.springframework.boot:spring-boot-starter-tomcat:${springBootVersion}”)



      * 必須在Application 加入以下的東西



        * @Bean


        * EmbeddedServletContainerCustomizer containerCustomizer()



      * 千萬記住在test那邊要把網址改成 http**s**://localhost:8443/ 而且在properties 裡面設定的 SERVER_PORT=9000 也沒有效果



    * 要加上login redirection 需要完成以下的修改: (參考以下修改  )



      * Add in build.gradle    



        * compile("org.springframework.boot:spring-boot-starter-security:${springBootVersion}”)



      * 需要新增一個class 繼承  WebSecurityConfigurerAdapter 來處理帳號與login/logout的問題與設定．



        * 必須要加上以下兩個設定，不然啟動的時候會有問題



          * @Configuration// Setup Spring Security to intercept incoming requests to the Controllers


          * @EnableWebSecurity




      * 在unit test的時候，需要注意以下的部分:



        * 根據目前的設定，任何的method都需要登入才能執行． 不然會跳出exception 可以由retrofit 接過來為 HttpStatus.SC_MOVED_TEMPORARILY


        * logout後應該也是要發生，exception 如果要試著去執行任何指令．




    * 課程以外，我把關於認證的部分做了一點小研究，主要就是根據不同的使用者權限來設定哪些方法可以執行～哪些會失敗 (參考[Github](https://github.com/kkdai/Java-Cloud-Learn/commit/289f4110625543d06c41a27151332f799d6dc850))



      * Security config 需要有以下: (refer: [https://spring.io/blog/2013/07/03/spring-security-java-config-preview-web-security](https://spring.io/blog/2013/07/03/spring-security-java-config-preview-web-security))  


```java
         @Autowired public void    
         configureGlobal(AuthenticationManagerBuilder auth)
    { 
    auth .inMemoryAuthentication() .withUser("user") 
    //#1 
    .password("password") .roles("USER") 
    .and() .withUser("admin") // #2 
    .password("password") .roles("ADMIN","USER"); 
    } 
``` 

  * 前面設定密碼是一樣的，就不重複敘述，這裡唯一要注意的，由於HasRoles 是使用字串，所以大小寫是有差別的．


      * 這邊也要注意一下 Authorize 跟Role 的差異： [http://docs.spring.io/spring-security/site/docs/3.1.7.RELEASE/reference/authz-arch.html](http://docs.spring.io/spring-security/site/docs/3.1.7.RELEASE/reference/authz-arch.html)



        * 這裡要注意: 



          * 根據定義 authorized(“ROLE_ADMIN”) == role(“ADMIN")


          * 但是你如果使用  hasRole(“ROLE_ADMIN”) 或是 roles(“ROLE_ADMIN”) 會出問題(tomcat runtime error)（但是其他任意字串是沒問題的)


          * **建議：  統一使用 roles(“ADMIN”)  與對稱的 hasRole(“ADMIN”)**




      * 更多參考



        * [http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html)


        * [http://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/config/annotation/authentication/configuration/EnableGlobalAuthentication.html](http://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/config/annotation/authentication/configuration/EnableGlobalAuthentication.html) 





  * 最後來到OAuth2，要完成OAuth2 需要有以下的步驟：



    * 先要在build.gradle 裡面加上:



      * compile("org.springframework.security.oauth:spring-security-oauth2:2.0.0.RC2”)    


      * compile("org.springframework.security.oauth:spring-security-oauth2-javaconfig:1.0.0.M1”)


      * compile("org.apache.httpcomponents:httpclient:4.3.4”)



    * 新增檔案去控制 UserDetails 與 UserDetailsService (參考: [這個改變](https://github.com/kkdai/Java-Cloud-Learn/commit/31279c12528aed8f662e6bdd6b9d5e526d58446c))


    * 針對Retrofit 的client 部分，需要implement 更多關於OAuth的部分，主要是：



      * 新增一個OAuthHandler 去負責 RequestInterceptor


      * 並且直接修改 RestAdapter.Builder 去新增OAuth的處理．




  * 在開發這種有client跟server的程式的時候，特別要注意的是如果發現有問題的時候，需要去釐清究竟是server還是client的問題．



    * 如果是學習這個課堂的範例的話，可以考慮跑跑課堂內附的client搭上你自己寫的server．有時候問題是出在client而不是你server有問題．


    * 如果是web server (Tomcat 或是 jetty ）可能比較麻煩～需要一步步把所加上的部分蓋掉來追蹤．


    * 千萬要注意 @PathVariable 跟  @RequestParam的差異，簡單解釋一下:



      * @PathVariable 是當你使用 GET Path 為參數比如說  video/2 


      * @RequestParam 雖然也走GET但是卻不是Path 而是會以其他方式傳遞．




  * 有個需要注意的部分是，關於回傳Http.Status Exception Code



    * 404:  Resource Not Found 



      * 可以直接用 throw new ResourceNotFoundException();



    * 400: Bad Request



      * 可以繼承 RuntimeExcption然後依照以下修改



        * @ResponseStatus(value = org.springframework.http.HttpStatus.BAD_REQUEST)


        * public final class BadRequestException extends RuntimeException  {   //  class definition}







**參考:**






  * 線上檔案比對 [http://www.diffnow.com/](http://www.diffnow.com/)


  * 關於HATEOS



    * Wiki HATEOS [http://en.wikipedia.org/wiki/HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)


    * Hatters gonna HATEOS [http://en.wikipedia.org/wiki/HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)



  * Spring Security InMemoryAuthorize 



    * [http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html](http://docs.spring.io/spring-security/site/docs/current/apidocs/org/springframework/security/config/annotation/web/builders/HttpSecurity.html)


    * [http://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/config/annotation/authentication/configuration/EnableGlobalAuthentication.html](http://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/config/annotation/authentication/configuration/EnableGlobalAuthentication.html)



  * 我把學習經過的Github



    * [https://github.com/kkdai/Java-Cloud-Learn](https://github.com/kkdai/Java-Cloud-Learn)





 
