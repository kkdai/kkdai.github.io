---
layout: post
layout: post
title: "[ELK Stack]利用logstash，elsticsearch與kibana來分析log"
description: ""
category: 
- ruby
- ruby on rails
- 學習文件
tags: []

---


![image](../images/2014/file-logstash-es-kibana.png)

**前言:**

本來只是跟幾個前同事聊天的時候聽到這個軟體，並沒有在意．結果之後就偶然有機會用的到．本來覺得還蠻複雜的，但是看完以下這篇影片好像就簡單多了．

[https://www.youtube.com/watch?v=Kqs7UcCJqu](https://www.youtube.com/watch?v=Kqs7UcCJquM)

**簡單的說:**

- **logstash**: 幫助你去收集各地的log或是資訊～並且根據你的格式，轉換成各種資料欄位．
- **elasticsearch**: 根據各種搜尋欄位來尋找資料．
- **kibana**: 視覺與圖形化的表示方式來顯示各種log，並且透過這個工具可以幫助你回答許多商業上的問題．比如說: 大部份使用者用那種裝置登入網頁？由哪個國家來？類似這些的問題．


接下來就打算要好好的研究一下，如何使用這套強大的工具．

<!--more-->

**安裝與執行:**

安裝上其實間單到個不行，所以不太需要敘述．唯一需要談的是要記得裝 Openjdk-7-jre ．

在執行Kibana的時候，我自己會一直出現連不到elasticsearch．後來我的解決方法是．先連一個不能連的位置．然後再連一次～就莫名其妙好了．


**Logstash 的簡單介紹:**

Logstash實在是一個功能很強大的工具，它主要能做的事情有:

- [抓取]由各種地方抓取相關的資料，不論是標準輸入(stdin)，檔案或是監聽一個連線都可以．並且可以把不同的資料來區隔開來作為之後用途．
- [篩選]抓取到資料之後，這時候可以選擇使用grok去拆解資料．比如說把一串非常長但是沒人看得懂的log拆解成各個相對應得資料欄位．
- [輸出]可以輸出到各種地方，不論是輸出到stdout印出來，或是直接丟給Elasticsearch拿來做分析用甚至可以直接輸出到資料庫．


**Elasticsearch的簡單介紹**

Elasticsearch是一個很有用處的資料搜尋引擎．只要能把資料餵給他(沒有限制是logstash的)，你就可以下參數去搜尋需要的資料．並且他會吐出相關的JSON回復．預設的連接埠是9200．

並且Elasticsarch支援RESTful的API，所以其實有很多的類似的extension 或是 plugin可以使用．

如果一直把資料餵給他，是可以把它當資料庫使用．

**Kibaba的簡單介紹**

Kibana連接著Elasticsearch之後能夠去搜尋與分析資料，並且把分析的資料用圖像的方式來表現．可以記住許多的分析參數來作為BI的用途．


**關於Logstash的進階使用**

- grok是一個強大的工具，可以免去你使用regular expression 的痛苦，不過它的語法還是需要習慣．它的用途主要是可以把一串長長的字串翻譯成你看得懂的JSON格式．不過前提是你得把資料格式敘述好～讓他知道如何對應．
- grok debugger 是一個好工具可以幫助你來找出你需要的match pattern. [https://grokdebug.herokuapp.com/](https://grokdebug.herokuapp.com/) 使用流程如下:
    - 先把你的Log RAW Data放到Discover的地方，可以幫你找出一些可能的pattern
    - 不然可以到Pattern去查查看常用到的pattern有哪些
    - 建立好基本的pattern之後，拿到Debugger那邊去一步步把它全部翻譯出來．
    
- 一些心得分享:
    - grok 的格式其實沒那麼鬆散，前面的資料還是得清楚敘述，連空格都不能少．後面可以用GREEDYDATA一起收下來．
    - 如果要做出 A or B的match需要使用以下的句子
        - **(?:A|B)**    
    - 要查詢已經建立好的grok pattern可以去這裡查詢 [https://github.com/elasticsearch/logstash/blob/master/patterns/grok-patterns](https://github.com/elasticsearch/logstash/blob/master/patterns/grok-patterns)
      

**結論**

ELK Stack (Elasticsearch + Logstash + Kibana)可以幫你解決一些問題如下:

- 太長的Log卻沒有一個方法可以整理跟分析它
- 太多來源的log，每次查詢一個問題需要打開三四個檔案，但是往往還是不知道原因．得要人工去查詢．
- 想要透過系統的log去做一些商業智慧(BI)的分析，但是又無從下手．

我覺得這套系統相當的簡單而易學，重點是也容易架設．我想可以作為伺服器要使用得時候，這個是必須要有的．


**參考資料**

- Logstash 相關:
    - Logstash Book [http://logstashbook.com/](http://logstashbook.com/) 
- Elasticsearch 相關
    - 初探Elasticsarch [http://ingramchen.io/blog/2014/06/elasticsearch.html](http://ingramchen.io/blog/2014/06/elasticsearch.html)
    - Slide: ELasticsearch 實戰介紹 [http://www.slideshare.net/gugod/elasticsearch-19877436](http://www.slideshare.net/gugod/elasticsearch-19877436)
- Logstash + Elasticsearch + Kibana 綜合簡介
    - 這篇蠻實用的 [http://www.slideshare.net/AmazeeAG/2014-0422-loggingwithlogstashbastianwidmercampusbern?qid=efe9afba-8831-4054-a290-d25183de38f2&v=qf1&b=&from_search=1](http://www.slideshare.net/AmazeeAG/2014-0422-loggingwithlogstashbastianwidmercampusbern?qid=efe9afba-8831-4054-a290-d25183de38f2&v=qf1&b=&from_search=1)
    - 蠻多基礎介紹 [http://www.slideshare.net/ae_bm/logstash-elasticsearch-kibana?qid=efe9afba-8831-4054-a290-d25183de38f2&v=qf1&b=&from_search=2](http://www.slideshare.net/ae_bm/logstash-elasticsearch-kibana?qid=efe9afba-8831-4054-a290-d25183de38f2&v=qf1&b=&from_search=2)
    - 對於三個工具的角色有一些介紹 [http://www.slideshare.net/nickchappell/pdx-devops-logstash-intro?next_slideshow=1](http://www.slideshare.net/nickchappell/pdx-devops-logstash-intro?next_slideshow=1)
    
    
    
