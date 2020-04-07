---
layout: post
layout: post
title: "[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (四)"
description: ""
category: 
- 網路課程筆記
tags: ["BigData", "Python", "Spark"]

---

![image](https://spark.apache.org/images/spark-logo.png)

## 前言:

最後一週，只有Lab．而最後一個Lab其實也接著之後的進階課程[Scalable Machine Learning](https://courses.edx.org/courses/BerkeleyX/CS190.1x/1T2015/info)． 所以最後一次的Lab開始介紹Spark關於Machine  Learning的部分．**喜好電影預測**是個很有趣，很實用的課題．


## 相關文章

edx 課程網址在這裡 [https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x](https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x)

- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (一)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-1/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (二)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-2/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (三)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-3/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (四)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-4/)


## Lab4 - Predicting Movie Ratings

![image](https://upload.wikimedia.org/wikipedia/commons/5/52/Collaborative_filtering.gif)

這一次的Lab主要是要做透過使用者對於喜愛電影的評分，加上從[MovieLen](http://grouplens.org/datasets/movielens/)上面超過10M資料抽出50萬筆一般大眾的評分資料．來預測你可能會喜歡的電影．運用的技術是所謂的["協同過濾"(Collaborative filtering)](https://zh.wikipedia.org/wiki/%E5%8D%94%E5%90%8C%E9%81%8E%E6%BF%BE)的方式來達成．  

**簡單的概念就是**: 如果一般人喜歡A電影的同時，大多也喜歡B電影．當你輸入你喜歡A電影的時候，系統就會預測出你可能也喜歡B電影．

上面的圖片可以有一個簡單的概述，也就是透過以下流程達成:

- 建立參考數值(也就是我們所認知的一般人喜好)
    - 計算每部電影的平均評分跟評分個數
    - 找出500個評分以上的資料．這些資料要來當作預測數值衡量基準．
- 訓練模型
    - 找出訓練的資料群組
    - 針對Matrix Factorization Model的資料類型，Spark提供[ALS.train](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.recommendation.ALS)的方式來train
    - 計算出預測出的RMSE(Root Mean Square Error)，也就是與大眾偏好的偏差值
        - 當預測數值越精確，RMSE會越趨近於零．反之，越趨近於一．
    - 透過rank的變化，挑選出最好的模型(RMSE最低的)來進行下一個階段
- 預測你可能會喜歡的電影
    - 輸入你對於電影的評分
    - 透過ALS.train預測結果
    

## 心得

關於最後的Lab，一開始還稍微卡住．不過跟一些人請教之後．一下子就把最後的Lab寫完．

我才發現其實Spark的使用上並不會困擾我，我反而是困擾在Python的Lambda運用．

因為題目裡面很多都只給一個<Fill In>． 但是當你不熟悉Lambda的時候，你就只會用很多行的方式來解決．

如果可以熟練使用Lambda的方式，更可以搭配著Spark的一些transform (map, filter, flatMap 或是 reduceByKey...) 來解決更多的問題．

有人推薦這本[Data Science from Scratch principles Python](http://www.amazon.com/Data-Science-Scratch-Principles-Python/dp/149190142X)似乎也蠻適合我這種對於Data Science完全沒有概念的人． 關於這本書的範例程式在[這裡](https://github.com/joelgrus/data-science-from-scratch)可以找到．

## 相關鏈結:

- [Movie Lens](http://grouplens.org/datasets/movielens/)
- [Wiki: 協同過濾](https://zh.wikipedia.org/wiki/%E5%8D%94%E5%90%8C%E9%81%8E%E6%BF%BE)
- [Wiki: Collaborative filtering](https://en.wikipedia.org/?title=Collaborative_filtering)
- [Book: Data Science from Scratch principles Python](http://www.amazon.com/Data-Science-Scratch-Principles-Python/dp/149190142X) 
- [Python Data Science Sample Code fro Book "Data Science from Scratch principles Python"](https://github.com/joelgrus/data-science-from-scratch)
