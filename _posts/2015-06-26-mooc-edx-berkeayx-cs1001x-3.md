---
layout: post
layout: post
title: "[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (三)"
description: ""
category: 
- 網路課程筆記
tags: ["BigData", "Python", "Spark"]

---

![image](https://spark.apache.org/images/spark-logo.png)


## 前言

剩下兩週了，不過家中寶貝誕生．必須要把時間做更好的調配，才能完成這個課程．

## 相關文章

edx 課程網址在這裡 [https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x](https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x)

- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (一)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-1/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (二)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-2/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (三)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-3/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (四)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-4/)


## Lecture 7 Data Management

- Data Clean: Deal with:
    - Missing Data
    - Entity Resolution
    - Unit mismatch
- 對於現實世界中數值可能出現的Dirty Data分為以下各種：
    - **失真(distortion)**: 當發現在連續作業中的某段資料不見的時候．
    - **選擇性偏差(Selection Bias)**:由於樣本的選擇不同，所以得到差異相當大的數值．比如說訪問政黨傾向不同得民眾．這裡有更詳細[資料](https://www.ptt.cc/bbs/NCKU-PH98/M.1200473358.A.9E4.html)．
    - **Left and Right Censorship**: 由於樣本不斷地來來去去，不能知道樣本是否有跑完所有採樣流程．
    - **相依的(Denpendence)**: 每個樣本應該是比次獨立，但是有一些沒有而造成資料的偏差．
- Data Gathering的重要性:
    - 資料在取得的時候存在許多發生錯誤的可能性，比如說硬體或軟體的問題或是欄位的問題..等等 若是能在取得資料的時候做一定程度的驗證，可以讓資料精確度更高．
- 其他名詞解釋:    
    - Big rot: 資料超過一定時間的沒有數值跟精準度．
- Data Delivery可能發生的問題與解決方式:
    - 資料遺失或是毀損:
        - 透過可信賴的傳輸方式或是透過Relay，也可以增加checksum來增加資料傳遞的可信任程度．
    - 資料來源的相依性與可信任程度:
        - 必須跟資料提供者有相當程度協調與協議．       


## Lecture 8 Exploratory Data Analysis and Machine Learning

- **敘述性統計(Descriptice Statistics):** 透過統計數量來對事件作敘述，比如說，本班的成績平均．
- **推論性統計(Inferencial Statistics):** 透過收集到的統計數字，對事件作一定的推論．比如說，選前問卷調查．

- 探索性數據分析(Explortory Data Analysis): 透過先行的探索來進行數據的分析，進行的活動可能有以下數種:
    - Visualizing the data distributions
    - Calculating summary statistics for the data
    - Examining the distributions of the data

- Rhine Paradox: 找了一千個人來猜連續十張為藍色或紅色的卡片．進而判斷出不能告訴超能力的人他有超能力的心理分析結果．
    - Rhine's Error: 但是真正的錯誤是，由於要猜測10張完全顏色一樣的機率為1 / 2^10 =>(1/1024) 也就是說一千個人要猜對一次也不到1個．其實要講解的是對於數據的不清楚造成錯誤的結論．

- **Spark Machine Learning Toolkit(mllib)**: 提供許多功能比如說分類與回歸分析或是一些數值分析的工具．


## Lab3

這次的Lab3更加深難度的要讓我們學習Text Analysis 跟 Entity Resolution並且有提到["詞袋模型"(Bag-of-words model)](http://terms.naer.edu.tw/detail/1679006/)，裡面有幾個比較需要注意的地方：

##### 關於 python dictionary 的 lambda:

這次比較困擾我的，其實是Python 的dict lambda

    //建立一個新的map，其結果是 key in MapA value 是 MapA[key]+ MapB[key]
    newDict = dict()
    for k in MapA:
        newDict[k] = MapA[k] + MapB[k]

    //可以用Lambda表示
    newDict = {k: MapA[k]+MapB[k] for k in MapA}

很多題目都只有給一個<FILL IN> 但是要做的事情太多，只得用lambda來思考．這個部分需要相當大量的腦力，蠻希望能有機會找找到更漂亮的解答．


#### 關於一些Apache Spark查詢的優化方式:

由於不少人的查詢方式有點慢，造成auto-grader 發生錯誤，於是學校方面發了一些聲明來指導大家正確的使用他：

- 關於找出最大的幾個數值的部分:


        #不要使用python的方式來排序，
        for i in vendorRDD.collect():
            measure len() to find biggest X

        #使用spark的action來找，放在記憶體跑會比較快．，
        vendorRDD.takeOrdered(helper function)

- 關於聯集合的部分:

        #不要使用python的集合合併
        unionRDD = sc.parallelize(A.collect()+B.collect())
        #直接使用 union
        A.union(B)

- 關於加總的部分

        #使用python的list家總是比較慢的．
        sum = sum(RDD.collect())
        #直接使用spark action裡面的加總
        RDD.sum() 

## 心得

關於["詞袋模型"(Bag-of-words model)](http://terms.naer.edu.tw/detail/1679006/)，一開始真的完全搞不懂這是在幹嘛．不過透過一步步地完成每一個小函式，最後組成完整的功能．也慢慢了解這個的原理．

這次由於中間卡著家中小寶貝的誕生，所以整個作業無法順利的全部做完．只能先把3/4的作業先弄完了． 不過這次的作業題目有夠難懂，主要時間都花在了解題目與思考提到到底要我們完成什麼．

最後，其實這週的lab3課程相當有趣，每個練習的排練方式也有提到一些些的原理．整個課程安排方式讓我回想到學習Scala的時候的學習脈絡，這大概也是學習FP的好處吧．


## 參考鏈結:

- [所谓探索性数据分析Exploratory Data Analysis更宜译作"试探性"](http://blog.sciencenet.cn/blog-350729-662859.html)
- [TF-IDF](https://zh.wikipedia.org/wiki/TF-IDF)
- 關於Bag-Of-Words
    - [Bag-of-Words 詞袋模型](http://terms.naer.edu.tw/detail/1679006/)
    - [非誠勿擾與bag of words](http://www.douban.com/note/333623111/)
    
