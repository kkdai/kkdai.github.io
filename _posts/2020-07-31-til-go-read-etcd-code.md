---
layout: post
title: "[TIL][Golang]  關於 etcd 的程式碼閱讀建議 -  segregrated hashmap 效能優化"
description: ""
category: 
- TodayILearn
tags: ["Golang", "OpenSource"]
---



## 前言:

每次在 Gopher TW 討論群裡面談到關於透過研究 open source 來了解架構的時候，我個人都很推薦 etcd 。原因如下：

- 工作單純 etcd 要完成事務相對單純。
- 基本 Client / Server 概念 （並且有許多工具）
- 具有 RAFT 分散式計算 HA 演算法

所以通常討論到該如何研究程式碼，我也很喜歡跟人家分享 etcd 。

程式碼鏈結: 

#### [https://github.com/etcd-io/etcd](https://github.com/etcd-io/etcd)



## 相關推薦：

所以這次討論的時候我也提出以下建議：

- 想搞懂 Raft 演算法～先看 https://github.com/etcd-io/etcd/tree/master/raft
- 工具怎麼寫就看 https://github.com/etcd-io/etcd/tree/master/tools
- client app 怎麼寫就看 https://github.com/etcd-io/etcd/tree/master/client

根據你的目的性來閱讀良好架構程式碼，就像讀一本好書一樣。 受益良多！



## 網友建議 segregrated hashmap 的效能優化：

根據網友 Naruto 建議：

`因為 bbolt db 的page 儲存區會出現效能問題，設計了一套 segregrated hashmap 去解決`

相關程式碼: [https://github.com/etcd-io/bbolt/blob/master/freelist_hmap.go](https://github.com/etcd-io/bbolt/blob/master/freelist_hmap.go)

快速搜尋一下，整理一下:

### segregated hashmap

由阿里巴巴發起的優化，根據他們事業業務所發現的效能瓶頸。

- 英文: [https://www.cncf.io/blog/2019/05/09/performance-optimization-of-etcd-in-web-scale-data-scenario/](https://www.cncf.io/blog/2019/05/09/performance-optimization-of-etcd-in-web-scale-data-scenario/)
- 中文: [https://www.infoq.cn/article/Dit9YCy2-ziDrLfFeQzq](https://www.infoq.cn/article/Dit9YCy2-ziDrLfFeQzq)

##### 成效很威

The new optimization reduces time complexity of the internal freelist `allocation algorithm in etcd from O(n) to O(1)` and the `page release algorithm from O(nlgn) to O(1)` , which solves the performance problem of etcd under the large database size. Literally, the etcd’s performance is not bound by the storage size anymore. The read and write operations when etcd stores `100GB of data can be as quickly as storing 2GB`.  This new algorithm is `fully backward compatible`.



找時間來研究一下...

