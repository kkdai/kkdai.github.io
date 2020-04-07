---
layout: post
layout: post
title: "[MOOC][Java][Programming Cloud Service] 額外學習 NoSQL，GAE與更多其它"
description: ""
category: 
- 網路課程筆記
tags: ["java", "spring", "mongoDB", "paas", "gae"]
---

主要的作業都做完了，不過接下來都是選修的部分．但是課程可是一點都不馬虎，又有mongoDB，又有GAE還有其他的部分．都是很值得學習的部分．
<!--more-->

**筆記:**

- 關於MongoDB的設定
  - 如果要使用local mongodb 一切都比想像中的簡單的多
     - 不需要帳號，與密碼
     - 建立mongoDB 與執行
       - brew mongodb
       - sudo mongd
     - 在Spring 裡面鏈結
       - add application.properties
       - spring.data.mongodb.host=127.0.0.1
       - spring.data.mongodb.port=27017
     - 這樣就可以了... 其他都不用改
     - 想要檢查是否有存取的話
       - mongo
       - use test
       - db.video.find()
- 在原來的Spring 加入MongoDB support 
  - 使用MongoRepository 換掉 VideoRepository
  - Gradle 增加:
     - compile("org.springframework.boot:spring-boot-starter-data-mongodb”)
   - 就這麼簡單，不過預設會寫到
      - Database: test
	  - Collection: Video
	  - 如果要使用自己定義的資料庫名字(database name)，需要增加以下的code (參考這裡)




<pre class="prettyprint">
class ApplicationConfig extends   AbstractMongoConfiguration 
    {
 
      @Override
      protected String getDatabaseName() {
        return "moocs";
      }
 
      @Override
      public Mongo mongo() throws Exception {
        return new Mongo();
      }
 
      @Override
      protected String getMappingBasePackage() {
        return"com.oreilly.springdata.mongodb";
      }
    }
</pre>    
   

**參考**：

- Spring Document
	- Spring getting start with MongoDB https://spring.io/guides/gs/accessing-data-mongodb/
	- Access MongoDB with REST https://spring.io/guides/gs/accessing-mongodb-data-rest/
	- http://docs.spring.io/spring-data/data-mongo/docs/current/reference/html/mongo.repositories.html
- JPA Repositories http://docs.spring.io/spring-data/jpa/docs/1.3.0.RELEASE/reference/html/jpa.repositories.html
