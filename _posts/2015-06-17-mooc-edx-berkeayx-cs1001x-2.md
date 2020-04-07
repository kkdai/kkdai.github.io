---
layout: post
layout: post
title: "[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (二)"
description: ""
category: 
- 網路課程筆記
tags: ["BigData", "Python", "Spark"]

---

![image](https://spark.apache.org/images/spark-logo.png)


## 前言

這一篇筆記主要是針對EDX上面的課程: [BerkeleyX: CS100.1x Introduction to Big Data with Apache Spark](https://courses.edx.org/courses/BerkeleyX/CS100.1x/1T2015/info)第三個禮拜的部分．剩下兩個禮拜而已... 加油!!

## 相關文章

edx 課程網址在這裡 [https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x](https://www.edx.org/course/introduction-big-data-apache-spark-uc-berkeleyx-cs100-1x)

- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (一)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-1/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (二)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-2/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (三)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-3/)
- [[MOOCS:edx]BerkeleyX CS100.1x- Introduction to Big Data with Apache Spark (四)](http://www.evanlin.com/mooc-edx-berkeayx-cs1001x-4/)


## Lecture 5

這一個章節主要是講解關於semi-structure資料結構與使用的方式． semi-structured資料主要講解就是一些類似XML或是Tag的資料結構．他有以下的缺點:

- 各個欄位間可能存在不一致
- 各個資料格式可能不盡相同(有的用英鎊有的用歐元)
- 各個欄位內容可能不一致 (walmart <-> wal-mart)

針對這樣的缺點，semi-structured資料使用起來雖然簡單，但是存在者某些程度上的挑戰與風險．  此外這一章節也討論了，資料讀取的速度與效能． 幾個重點紀錄一下:

- 對於壓縮格式的檔案讀取，讀取會比寫入快，
- 而Binary IO會比 Text file 快，但是Text file 壓縮後比較小(壓縮比大)
- [LZ4](https://en.wikipedia.org/wiki/LZ4_(compression_algorithm))壓縮比跟讀取速度都相當優秀．

## Lecture 6

這一張主要講解的是Structured Data，也就是主要講解關於RDBMS相關的部分．這裏主要提到就是join與各種join的計算方式． 主要要注意的就是要如何的應用 spark join．


## Lab2

比起Lab1來說，Lab2的份量實在是多了很多． 案例是分析一個月的Apache Log 透過Spark的一些操作可以分析一些有用的數據． 由於資料量也變大，這次的每個小案例，需要花上比較多的時間來執行．

### 關於regular expression的grouping

這邊有提到regular expression 的grouping，方法就可以幫你把一行字只要符合的狀況下就會切成一個Array來處理．
這裏用一個簡單的案例:

        test_string = "[aa bbb ccc]"
        match = re.search('^[(\S*) (\S*) (\S*))]', test_string)
        #match[1] = aa, match[2] = bb, match[3] = ccc


### 常用到的幾個計算方式

這裏有一些常用到的計算流程很適合做筆記：

#### 紀錄某個資料出現個數

假設要計算某個錯誤資料每天出現幾次，計算方式可以如下：

    #將錯誤資料變成tuple (2015-06-15, 1), (2015-06-16, 1)....
    errDateCountPairTuple = errRecords.map(lambda data: (data.datetime, 1) )

    #將錯誤資料透過reduceByKey之後，然後將每一個加總．
    errDateSum = errDateCountPairTuple.reduceByKey(lambda a,b: a+b)
    
    #排序
    errDateSort = errDateSum.sortByKey()

#### 如果要畫圖得時候

    #X軸
    errDateSort.map(lambda (key, value): key).collect()
    #Y軸
    errDateSort.map(lambda (key, value): value).collect()


#### 想要取得top5

    #想要數字多的在前面，使用-1 如果要少的在前面 就使用+1
    errDateSort.takeOrdered(5, lambda s: -1 * s[1])


這大概是比較常用的幾個，此外也有幾個function 需要注意． 不外乎是 `distinct()` 跟 `cache()`， 如果出現任何問疑的時候，也建議趕快把資料印出五個來debug

    #檢查如果有誤
    errDateSort.take(5)    
            

## 心得整理:

這一次作業的資料量已經有點大，而且裡面的題目也有點多． (大約有20~30題) 每一個運算都要跑好久才有結果．

以下是我的心得:

1. 跑很久... 
2. 錯了再一次. 又要很久...   
3. 沒想好... 就跑... 真的要很久..... 
4. 不小心搞掛系統... 全部重跑.. 會更久......﻿

此外，由於使用ipynb來單步執行作業，繳交作業前也要確認你的ipynb沒有任何的warning跟error．就算是之前不小心跑出來也要讓他都成功才能繳交作業．


## 參考鏈結:

- TBD
