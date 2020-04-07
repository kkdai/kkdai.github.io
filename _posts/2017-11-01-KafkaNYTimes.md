---
layout: post
title: "[Kafka][好文推薦] Publishing with Apache Kafka at The New York Times"
description: ""
category: 
- 好文分享
tags: ["software", "kafka", "cloudcomputing"]

---

![](https://www.confluent.io/wp-content/uploads/kafka-based-publishing-archi-min.png)

### 原文: [confluent: Publishing with Apache Kafka at The New York Times](https://www.confluent.io/blog/publishing-apache-kafka-new-york-times/) 


## 起因:

這篇文章一開始是因為在 [SoftwareEngineer Daily](https://softwareengineeringdaily.com/2017/10/30/kafka-at-ny-times-with-boerge-svingen/) 的 Podcast 聽到的．整個內容相當的有趣，於是回頭將這篇文章仔細的看了一下，發現這篇文章其實可以解決許多具有舊有大型系統需要做新舊系統切換與整合，就很適合看看這篇文章所提供的經驗與方式．

## 文章內容:

### NY Times 面對的問題

身為一個具有 161 年的老公司（大部分是紙本），並且具有 21 年的線上內容的新聞媒體公司．他們遇到的問題，有以下的部分：

有多種內容管理系統(CMS) 需要將資料送到些後續處理的服務上，比如說:

- 內容搜尋引擎
- 個人化的內容配對
- 相同的類別資料
- 甚至是一些其他的後續處理資料服務

而在資料的來源上，其實也是具有多個資料來源．分別是：

- 不同的 CMS 系統（因為可能有多個出版媒體資料）
- 歷史資料（透過拍照存檔的舊新聞資料）

就如同一開始的那張圖依樣，問題就是要面對一個 n:m 的狀態上，該如何設計這樣的架構呢?

### 舊的方式: RESTful API 來呼叫

![](https://www.confluent.io/wp-content/uploads/kafka-base-archi-min.png)

就像這張圖一樣，起出許多人也是這樣來實作你們的內部系統．透過 RESTful API 的方式來串起多個資料來源系統與需要串接得多個服務系統．

但是這樣的串接方式會有以下的問題: （一個 `n:m` 的關係圖)

- 彼此的團隊都要面對各種不同的 API 文件與各個團隊不同的使用情境
- 當 API 有任何變動的時候，影響層面就變成所有的來源團隊都要變動
- 提供 API 的團隊往往設計出比較複雜的流程提供給呼叫端，讓呼叫端需要做相當多的資料處理．

### 解決方式: 透過一個 Log-based architecture

這邊建議的方式，就是透過一個 Gateway 來將資料放在 Kafka ，這樣右端的資料處理部分就只要去 Kafka 拿取到相關資料後自己做相關的資料處理． 這樣的處理有以下好處:

- 資料提供端（左方）的人只要專心提供 Kafka 中間層定義的資料結構．
- 資料處理端（右方）的人可以將 Kafka 拿取來的資料自行處理成需要的格式．
- 進階的變化上，如果資料處理端需要更多的資料處理，還可以寫在 Kafka Stream 上面直接處理過後轉到自己的 topic 



TBC