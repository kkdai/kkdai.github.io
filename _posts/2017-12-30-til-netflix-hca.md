---
layout: post
title: "[TIL] Netflix: Heterogeneous Cluster Allocation"
description: ""
category: 
- TodayILearn
- Kubernetes
tags: ["TIL", "netflux", "consistenthashing", "algorithm"]
---

![](https://cdn-images-1.medium.com/max/1600/1*vfOeuLW5yh_9x6OHSaSbpA.png)

## [原文:  Netflix: Distributing Content to Open Connect ](https://medium.com/netflix-techblog/distributing-content-to-open-connect-3e3e391d4dc9)  

最近都在忙著趕公司專案，這篇文章讀了好久．

Netflix  最近寫了這篇技術文章來解釋他們如何面對大量影片下載需求的 Load Balance 的方式 : Heterogeneous Cluster Allocation (HCA)

一般來說我們都會使用 Consistent Hashing 的方式來處理伺服器分配流量的方式． 這也是最為人熟知的 Load Balancing 的方式． 但是 Netflix 提出了有趣的想法是，作為 Load Balancing 的時候，我們都是把伺服器當作是同質的 (註: 同樣速度，同樣硬碟大小）  

但是如果其實伺服器可能是異質的(Heterogeneous) ，舉例來說: 有的一般硬碟  100TB  但是 throughput 比較低 ，另外則有速度快的 SSD 50 TB ，但是 throughput 相對來說就比較高．這樣的分配法則下，原來的 Consistent Hashing 就無法滿足這樣的方式．

 於是 Netflix 他們使用 HCA 的方式來達到更適量的分配．針對這兩種異質的方式，Netflix: Heterogeneous Cluster Allocation (HCA) 作法如下:

- 在兩種異質的機器上，各自給予 content 相對的權重 (weight)．權重的評比來自於 content 受歡迎程度

- 兩種機器各自有不同的 cutoff (也就是 threashold) ，來決定該 content 是否受歡迎． (ex: 40Gbps, cutoff: 0.3,  100Gbps 0.28..)

透過這樣的調整，可以確定接下來 content 會被放在哪裡．也能夠更有效率的分配流量．並且也驗證了在負載高的時候，更能讓不同的需求讓不同的伺服器來服務．
