---
layout: post
title: "[論文導讀]The Case for Learned Index Structures (一)"
description: ""
category: 
- TodayILearn
tags: ["Paper", "MachineLearning", "B-tree", "BloomFilter"]
---



![](https://adriancolyer.files.wordpress.com/2018/01/learned-index-fig-1.jpeg?w=520&zoom=2)

## 論文原文: [The Case for Learned Index Structures](https://arxiv.org/abs/1712.01208)

### **Morning Paper Reading**: [part1](https://blog.acolyer.org/2018/01/08/the-case-for-learned-index-structures-part-i/), [par2](https://blog.acolyer.org/2018/01/09/the-case-for-learned-index-structures-part-ii/)

## 緣起：

剛好最近有幾次機會可以去工研院開會的路上，在高鐵的路途上可以好好的來欣賞這篇文章． 這篇文章是由 Google Brain 的大神 Jeff Dean 連署的論文之一．講的是透過 NN 的方式來讓大家熟知的 B-Tree, Hashing table 甚至是 Bloom Filter 更有效率...

2018 年第一篇好好閱讀的論文，當然要獻給有深度而且相當有趣的這篇文章．[The Case for Learned Index Structures](https://arxiv.org/abs/1712.01208) ．主要的原因有以下:

- 這篇是講解一個新的機器學習的新領域（至少是相當有趣的觀點）
- 雖然有些限制跟品質的降低，但是讓我們對於 AI/Deep Learning 有了一個新領域的想法...
- 這篇是被稱為[世界上最聰明的人](https://www.quora.com/What-are-all-the-Jeff-Dean-facts) Google Brain 的主持人 - Jeff Dean 的論文( 編按: [這篇 Quora 有許多關於 Jeff Dean 的敘述文，相當的有趣](https://www.quora.com/What-are-all-the-Jeff-Dean-facts)  XD)

Jeff Dean 是誰? 快來看他的 Quora<https://www.quora.com/What-are-all-the-Jeff-Dean-facts>

\- 他看得懂, 也寫 Binary code
\- 他的 PIN code 是 Pi 末四碼

## 先講講 B-Tree 

B-Tree 是大家相當熟知的資料結構，在此僅列出幾個需要知道的．

- 時間複雜度 $$ O(log n) $$  (Balanced B-Tree)
- 需要空間(cache) 代表存多少資訊在 B-Tree ．當資料不在 cache 中，代表需要重新跑一次 re-balanced
- B-Tree traversal 無法分散式處理 (透過 GPU 來加速)

## 再來談 Learning Index Tree

回過來講 B-Tree Index 你可以把一個數值輸入 B-Tree ，透過搜尋過後可以傳回一個 Index (可能有 re-balanced)．

換個角度，如過透過 NN (Neural Networking) model 的學習將一個數值輸入後，來預測 (predict) 它可能的索引位置 (index) ．那麼我們就稱這個為 Learned Index 

這邊有一些你需要知道關於 Learned Index 的部分:

- 由於直接運算，所以時間複雜度相當的低:  $$O(1)$$ 
- 由於透過 NN 來運算，可以很輕易透過 GPU 來加速運算
- 空間要求相當的少 
  - 不像 B-Tree Index 需要一定的空間來儲存目前已知的數值來加速． （根據文章: cache size 128 是最快的)

## 關於 CDF (Cumulative Distribution Function)

![](https://adriancolyer.files.wordpress.com/2018/01/learned-index-fig-2.jpeg?w=520&zoom=2)

B-Tree 就可以當成是一個透過 Key 值來轉換到 Index (Pos)的方式． 那麼也可以當成是透過 Key 的分佈來預測 (predict) 位置．
$$
p = F(Key) ∗ N
$$

##  Tensorflow 來做 CDF 與傳統 B-Tree Index 的效能比對

- **來源資料**: 200M 筆的 Web Log
- **Activation fnction**: **ReLu**
- 32 個 Neural per layer

透過 Tensorflow 來實作為 Naive Learning Index 與 B-Tree 的效能比對． **B-Tree 快上 2~3 倍**，論文提出以下理由:

- Tensorflow 設計是為了處理大量的數據，對於 200M （相對小) 他本身的處理效率有相對的消耗
- B-Tree 的結果會 `overfit` 也就是說完全以輸入的資料來作為 B-Tree 的計算與樹狀的建置． 但是相反的 CDF 是透過 NN 來計算"可能性"的位置．
- B-Tree 需要比較大的 Cache (比較表內使用 128 的 cache 效能最佳) ，而 CDF 透過 NN 來學習不需要儲存之前的資料．（不過兩者都需要 re-train (CDF) 與 re-balanced(B-Tree) )

## 參考文章:

- [arXiv: The Case for Learned Index Structures](https://arxiv.org/abs/1712.01208)
- [Morning paper: The case for learned index structures – part I](https://blog.acolyer.org/2018/01/08/the-case-for-learned-index-structures-part-i/)
- [Morning paper: The case for learned index structures – part II](https://blog.acolyer.org/2018/01/09/the-case-for-learned-index-structures-part-ii/)