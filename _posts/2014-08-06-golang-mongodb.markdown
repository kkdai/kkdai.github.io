---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-06 16:16:30+00:00
slug: golangmongodb-%e7%b7%b4%e7%bf%92%e4%b8%80%e4%b8%8bmongodb-%e8%88%87golang-%e7%9a%84%e5%9f%ba%e6%9c%ac%e9%80%a3%e7%b5%90
title: '[Golang][MongoDB] 練習一下MongoDB 與Golang 的基本連結'
wordpress_id: 1480
categories:
- Golang
- mongodb
---

## 前言：

[自學Golang](http://www.evanlin.com/blog/?cat=59)也到了現在，除了繼續深究Golang在許多層面的應用之外．也必須學習一些新的資料庫．  
想了一下，就決定拿經常在研討會裡面看到的NOSQL (並不是 NO - SQL 而是 Not-Only SQL )的資料庫 [MongoDB](http://www.mongodb.org/)來練習一下．  
[MongoDB](http://www.mongodb.org/)比起一般的[RDBMS](http://en.wikipedia.org/wiki/Relational_database_management_system)而言，應該算是比較容易了解的．而且對於從手機程式設計開始學習的人可能會更容易上手．  
因為 [MongoDB](http://www.mongodb.org/)本身不斷圍繞著一個資料結構，就是 [JSON](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&cad=rja&uact=8&ved=0CDcQFjAC&url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FJSON&ei=fE3iU9-PCY3n8AWX14GwDQ&usg=AFQjCNHfk8CeJn25-S_gvF4dnY6ZaKxg4g&sig2=1sQkrCvpdSIkilNaxpX27g&bvm=bv.72197243,d.dGc)．


**[MongoDB](http://www.mongodb.org/):**




## 關於:

本身概念相當的簡單，主要分為Database(資料庫)， Collection (中文該怎麼翻比較順呢？) 還有就是最基本的資料原件 Document (文件)．  
這樣如果還不是很容易了解，換這樣來換吧:  

  * DB一樣就是原先RDBMS裡面的資料庫
  * Collection 可以當成一個個的Table
  * Document 可以當成一個個的Record

這樣並不是最好的對比，不過這是一個概念上可以讓人一開始馬上進入的方法．

## 安裝或使用：

如果要自己架設MongoDB(Mac OS)可以[參考一下這裡](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)，或者是去[ MongoDB HQ](https://app.mongohq.com)上面申請一個免費的MongoDB (我目前是申請一個免費的資料庫來測試）




使用上需要注意的特點:

  * 由於你的資料欄位最基本是JSON，所以每個Collection 裡面並沒有固定的欄位格式．可以前兩個documents 是有五個欄位，後面卻不是．
  * 關於查詢
    * equal 比較簡單，就是一般JSON語法  `{ price: 40 }` 查詢價錢40元的
    * greater than (大於) 或是 Less than (ls）的時候需要使用   `{ price: { $gt : 40} }` (查詢價錢 大於 45)  (要查小於就是  `$lt` ）

### Golang上面大家推薦的MongoDB Driver — [mgo](http://labix.org/mgo) 

其實安裝跟使用相當的簡單，裡面也提供很多好用的方法．不過我在學習的途中有遇到一些問題希望可以幫助大家．

  * Document 的type struct 裡面的資料名稱，不可以全部小寫(lowercase)不然會出現insert到collection 裡面變成空白（empty document except ObjectID)．
    * 我本身有在追他的source code但是還看不出是在哪裡有問題．
  * (承上)雖然變數名稱必須有大寫(uppercase)但是到了[MongoDB](http://www.mongodb.org/)裡面又得用全部小寫(lowercase)．在一般的資料庫操作中，這是可以理解的．
  * 關於Query Criteria 的參數
    * 要下greater than 必須用 `$gt`  範例：  `bson.M{"price": bson.M{"$gt": 40}}` 找出價錢大於40





基本的查詢與操作都會了之後，接下來要弄到Heroku上面去跟server搭配了．
範例[可以到這裡去找 Github](https://gist.github.com/kkdai/813f1aaf7cd58487472a) …..

### 參考：

  * [http://blog.joshsoftware.com/2014/02/28/a-simple-go-web-app-on-heroku-with-mongodb-on-mongohq/](http://blog.joshsoftware.com/2014/02/28/a-simple-go-web-app-on-heroku-with-mongodb-on-mongohq/)
  * [http://www.codedata.com.tw/database/mongodb-tutorial-1-setting-up-cloud-env/](http://www.codedata.com.tw/database/mongodb-tutorial-1-setting-up-cloud-env/)
  * [http://www.codedata.com.tw/database/mongodb-tutorial-2-query-language/](http://www.codedata.com.tw/database/mongodb-tutorial-2-query-language/)
  * [http://docs.mongohq.com/getting-started/mongohq.html](http://docs.mongohq.com/getting-started/mongohq.html)
  * [https://gist.github.com/border/3489566](https://gist.github.com/border/3489566)


