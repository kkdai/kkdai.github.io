---
layout: post
title: "[TIL]關於BlockChain的學習整理"
description: ""
category: 
- TodayILearn
tags: ["TIL", "bitcoin", "blockchain"]

---

![](https://i.ytimg.com/vi/MnT9d_rgo3o/hqdefault.jpg)

## 前提:

主要是在Line上面的討論，有人覺得像是Bitcoin中blockchain把資料全部分散放在各個client裡面是很不好的做法．這樣所有資料都redundancy．

於是再把一些相關spec拿出來看.. 快速整理一下..

<br><br>

## Blockchain 架構速記:

#### Bitcoin 最好的 blockchain 1.0 應用

喧賓奪主的應用，大概非Bitcoin莫屬． 沒太多人(一般民眾)注意到blockchain的精髓． 只記得這個應用.. 不過隨著幾個大廠商(ex: IBM) 也在開發自己的blockchain 應用．  這一塊早已在新創產業不是話題了...


bitcoin 被稱為是blockchain 1.0的應用，而現階段在新創產業與一些大公司在推廣的就稱為是blockchain 2.0 (smart contract) ．


#### 簡單架構

- Bitcoin wallet:
	- 一個記錄所有交易紀錄(以下皆成為block)的資料庫(使用[Google LevelDB](https://github.com/google/leveldb))
	- 每筆交易紀錄(block)平均大小是250bytes，其格式包括了:
		- Block Size: 
			- 4 bytes, 來敘述block大小．
		- Block Header:
			- 80 bytes, Block header 包括 version, previous hash, timestamp.. (還有其他的)
		- Transaction Counter:
			- 1~9 bytes (可變) 有多少交易在這個block.
		- Variable:
			- 大小不定, 交易內容..
	- 每筆記錄(block)與前一筆是串接在一起的(ordering)，透過Hash value作為唯一的識別(using SHA256)．

- Bitcoin miner: (所謂的挖礦機)
	- 負責尋找最新的block hash value，並且確保該hash value 還沒有被產生過．
	- 產生block會先加在最後，並且獲得一定的bitcoin作為獎勵．由於SHA256有其限制，所以能產生的bitcoin被限制在**21000k bitcoin**．
	- 取得新的Hash後，需要以下的兩者擇一成真，才能獲的bitcoin:
		- 取得大多數的wallet的同意(大於1/2 的 computing power)
		- 連續取得6個新的Hash (表示你的機器速度大於全世界的miner六倍以上)

<br><br>

## Bitcoin的特性與展望:

### 特性:

- 去中心化
- 安全(透過其他P2P的審議機制)
- 所有端點會記錄大部分資料(或全部資料)

<br><br>

## 未來展望:

除了金流的bitcoin之外，可能有以下的應用:

#### 電子簽約(Smart Contract)

- 將所有的簽約內容當成錢幣來存放，所有相關人都必須存放所有內容．
- 存放雙方資料，商品名稱與單位量．


挑戰:

- 如何計算營收？
- 如何避免透露過多商業資訊?

#### 證券交割

將股票單位作為blockchain 交易的單位．

#### 遊戲幣

這邊沒有太多特別地方，就是把遊戲中的貨幣改成透過blockchain．可以讓民眾私下交易遊戲幣．


#### 專利與商品證明

[bitproof.io](https://bitproof.io/) 想到透過blockchain來保護智慧財產權．他可以透過檔案(將你的專利或是文章作成檔案後，上傳來取得特定的標記)，透過該標記來證明你是第一版的擁有者．
	
	
## 其他:

朋友在討論中談到:

"Blockchain 機制會造成資料的重複嗎?"

經過我尋找，一些資料如下


1. 每筆交易資料 平均是 250 bytes (包含多筆交易資料)  平均每筆交易資料為  26 bytes  (參考[bitcoin wiki](https://en.bitcoin.it/wiki/Transaction))
2. 透過Google LevelDB 大小會被壓縮..
3. 其實儲存可能不會超過一半以上，因為機制上只要一半以上通過就可以． 不過當網路順暢時還是會儲存全部． 
	
## 相關鏈結:

- 	[Google LevelDB](https://github.com/google/leveldb)
-  [How bitcoin use LevelDB](https://www.quora.com/How-is-Bitcoin-using-LevelDB)
-  [押宝比特币？错，IBM看重的只是Blockchain未来](http://junstapo.baijia.baidu.com/article/322495)
-  [Amazon Book: Mastering Bitcoin: Unlocking Digital Cryptocurrencies ](http://www.amazon.com/Mastering-Bitcoin-Unlocking-Digital-Cryptocurrencies/dp/1449374042)
-  [bitproof.io透過bitcoin的專利機制](https://bitproof.io/) 
-  [Bitcoin Wiki: Transaction](https://en.bitcoin.it/wiki/Transaction)