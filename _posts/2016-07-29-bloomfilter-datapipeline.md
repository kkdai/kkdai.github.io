---
layout: post
title: "[Golang] FOSDEM 2016: Building Data applications with Go: from Bloom filters to Data pipelines 心得"
description: ""
category: 
- golang
tags: ["bloomfilter", "vagrant", "docker", "DCOS"]

---

![](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ac/Bloom_filter.svg/360px-Bloom_filter.svg.png)

## 前提:

這一篇主要是看[FOSDEM 2016 影片](https://archive.fosdem.org/2016/schedule/event/data_apps/)的簡單心得（投影片在[這裡](http://www.slideshare.net/khomenko1/building-data-applications-with-go-from-bloom-filters-to-data-pipelines-fosdem-jan-31-2016)），順便把之前學的 Bloom Filter 複習一下． 這裡有我之前寫的[程式](https://github.com/kkdai/bloomfilter)．


## 心得 

這篇文章主要都介紹透過　Golang 來開發一個 Data Application ． 從 Bloom Filter 到 [Count-Min](https://en.wikipedia.org/wiki/Count%E2%80%93min_sketch) （透過 Hash 方式來存放資料，主要是記錄有出現多少次），到了 HyperLogLog （

## 關於 Bloom Filter

#### 這是什麼?

Bloom Filter 是一個資料結構．主要是拿來能夠快速的確認一個數值有沒有存在的資料結構．具有以下特性:

- 極小使用空間（由於不存在原本的 Value ) 只需要儲存 `k` 個資料結構就好．
- 具有 `可以判斷該數值絕對不存在` 效率 ( 也就是不具有 False Nagative ，但是具有 False Postive )． 搜尋時間複雜度: $$O(n)$$

####  使用場景:

- 爬蟲可以記錄該網址是否有爬過
- Google 惡意 URL 判斷
- Canssandra 判斷該 Partition 是否有存放該數值



## 參考鏈結

- [FOSDEM 2016: Building Data applications with Go: from Bloom filters to Data pipelines](https://archive.fosdem.org/2016/schedule/event/data_apps/)
- [Wiki: Bloom Filter](https://en.wikipedia.org/wiki/Bloom_filter)
- [https://github.com/pmylund/go-bloom](https://github.com/pmylund/go-bloom)
- [https://github.com/willf/bloom](https://github.com/willf/bloom)
