---
layout: post
title: "[Golang][Notion] 如何透過 Golang 來操控 Notion DB 當成線上資料庫"
description: ""
category: 
- Golang
tags: ["Golang", "GoogleGemini", "LLM"]

---

![Notion Databases: An advanced tutorial on Notion's game-changing feature](../images/2022/OIP.jpeg)



# 前提

在撰寫許多 Side Project 的時候，除了網路服務伺服器之外，最困擾的大概就是資料庫的問題。雖然之前我的文章 [[學習心得\][Golang] 把 Github Issue 當成資料庫來用](https://www.evanlin.com/go-github-issue/) 曾經教過透過 Github Golang API 來將簡單的一些資料放在 Github Issue 上，但是如果資料格式比較複雜的時候。可能就會需要透過類似資料庫格式的儲存體來處理。 偏偏許多線上資料庫都是算時間與用量，對於想寫一些有趣的 Side Project 卻沒有那麼友善。

本篇文章將使用 [Notion Database](https://www.notion.so/help/guides/creating-a-database) 作為資料的儲存體，並且透過 Golang 去查詢，插入相關的資料處理。 本篇文章也會從如何設定一個 [Notion Integration](https://developers.notion.com/docs/create-a-notion-integration) 開始教導，讓你透過 Golang 來操控  [Notion Database](https://www.notion.so/help/guides/creating-a-database)  沒有任何痛苦。



# 首先了解 Notion Database 

<iframe width="560" height="315" src="https://www.youtube.com/embed/O8qdvSxDYNY?si=HpjDcPf42mp5TqTR" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

以上是一段 [Notion 官方教學影片 Notion Database](https://www.youtube.com/watch?v=O8qdvSxDYNY) 裡面有提到如何建立一個 Database ，並且有稍微解釋：

- Create -> 選擇 Database 欄位中 ->  Table 

## 使用 Notion Database 的好處:

![image-20240115211147923](../images/2022/image-20240115211147923.png)

- Notion Database 支援相當豐富的格式，並且有很漂亮的視覺化介面。
- 並且 [Notion Database](https://www.notion.so/help/guides/creating-a-database)  支援多種格式： Table, Board, Calendar, List,  and Gallery
- 除了



# 參考資料：

- [[學習心得\][Golang] 把 Github Issue 當成資料庫來用](https://www.evanlin.com/go-github-issue/) 
- [Notion Database](https://www.notion.so/help/guides/creating-a-database) 
- [Build your first Notion Integration](https://developers.notion.com/docs/create-a-notion-integration)
- [Notion 官方教學影片 Notion Database](https://www.youtube.com/watch?v=O8qdvSxDYNY)
