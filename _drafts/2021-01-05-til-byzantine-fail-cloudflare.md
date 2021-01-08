---
layout: post
title: "[TIL][讀後心得] 分散式系統現實世界的拜占庭將軍問題 (note from CloudFlare - A Byzantine failure in the real world)"
description: ""
category: 
- TodayILearn
tags: ["TIL", "distributed_system", "etcd", "golfing"]
---



![](https://miro.medium.com/max/700/1*kJJpYLrKZ5hgByA-q3Zkjw.jpeg)



# 前提





## 什麼叫 Byzantine failure

在分散式系統中，不同電腦之間會相互的溝通作為「Consensus Communication」的資料確認流程。 需要不同電腦間，回報自己將要做的事情，或是進行 leader 投票的流程。 如果這時候發生了有一台電腦，跟某些團員講 A 跟另外一群團員又講 B 的狀況，造成整個團體無法達成一制性，或是達成了非預期狀態，就被稱為是 Byzantine Failure 。 許多的狀況下， Consensus Algorithm 舉凡 Paxos 跟 Raft 都會先假設 Byzantine Failure 是不存在的，因為這個問題會將 Consensus 複雜度又提升到另外一個境界。

#### 參考文章:

- [Wiki 拜占庭將軍問題](https://zh.wikipedia.org/wiki/%E6%8B%9C%E5%8D%A0%E5%BA%AD%E5%B0%86%E5%86%9B%E9%97%AE%E9%A2%98)

## 關於 CloudFlare 的復原機制

探討比較複雜的問題之前，其實這篇文章還有一個有趣的角度可以觀察。 就是如何透過 CloudFlare 的 Incident Report 來查看他們針對系統維護上有那一些備援機制。 

### 服務備援機制

- 每一個服務都是一系列的 Rack Servers
- 每台機器有兩個 switches 
- 每個機器主機架有兩個以上的電源供應設備
- 每個 server 都使用 RAID-10 的備份機制 ([也就是 RAID 1 + RAID 0 的備份機制](https://en.wikipedia.org/wiki/Nested_RAID_levels#RAID_10_(RAID_1+0)))
- 每一個 Rack 至少都是三台機器以上。



## 發生的問題

![](https://blog.cloudflare.com/content/images/2020/11/image1-20.png)

（**圖片解釋**： 左上角為 Server 1, 右側為 Server 2，下方為 Server 3 也是 Leader )

- 由於 Server 1 到 Server 2 的網路發生問題。
- 造成 Server 1 與 Server 2 發生不同步的資訊問題。
  - Server1 認為 Leader (Server 3) 已經下線
  - Server 2 認為 Leader 是正常運行。
- 也是因為這樣的原因， CloudFlare 將這個問題在為 Byzantine Failure









# Reference

- [Cloudflare Dashboard and Cloudflare API service issues](https://www.cloudflarestatus.com/incidents/9ggr0k6dwzwg?_ga=2.204546386.37818800.1609918736-1905359649.1609918736)

- [A Byzantine failure in the real world](https://blog.cloudflare.com/a-byzantine-failure-in-the-real-world/)

- [Raft does not Guarantee Liveness in the face of Network Faults](https://decentralizedthoughts.github.io/2020-12-12-raft-liveness-full-omission/)

- [wiki: 拜占庭將軍問題](https://zh.wikipedia.org/wiki/%E6%8B%9C%E5%8D%A0%E5%BA%AD%E5%B0%86%E5%86%9B%E9%97%AE%E9%A2%98)

- [Raft lecture (Raft user study) by Diego Ongaro](https://www.youtube.com/watch?v=YbZ3zDzDnrw)