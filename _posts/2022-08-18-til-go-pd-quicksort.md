---
layout: post
title: "[學習文件][Golang] Go 1.19 的 Sort 變快了 "
description: ""
category: 
- 學習文件
tags: ["Golang", "Quicksort", "Algorithm"]
---

![image-20220819163803756](../images/2021/image-20220819163803756.png)

## Go 1.19 的 Sort 變快了 

Go 1.19 的 Sort 已經從 QuickSort 換成跟 #Rustlang 還有 C++ #Boost 一樣的  Pattern-Defeating #Quicksort

想知道多關於 PD Quicksort 可以[參考這篇字節跳動團隊 “”打造 Go 语言最快的排序算法""(簡中)](https://blog.csdn.net/ByteDanceTech/article/details/124464192)

## 關於 Pattern-Defeating Quicksort 解釋

強烈建議，這個影片可以看。

<iframe width="560" height="315" src="https://www.youtube.com/embed/jz-PBiWwNjc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 快速解釋什麼是 PD Quicksort？ 

- 從 QuickSort 優化，最佳狀況從 O(n log n) --> O(n)
-  最差狀況 O(n log n) 

### 假設前提設定

<img src="../images/2021/image-20220825004251117.png" alt="image-20220825004251117" style="zoom:33%;" />

- In-memory random access
- Cheap comparison / moves

### Pivot 挑選很重要

<img src="../images/2021/image-20220825004022080.png" alt="image-20220825004022080" style="zoom:33%;" />

### 根據不同場景，有其他更好建議

<img src="../images/2021/image-20220825003748676.png" alt="image-20220825003748676" style="zoom:33%;" />

## 相關文章：

- 圖片來自投影片(日文)： [https://speakerdeck.com/po3rin/go1-dot-19decai-yong-sareta-pattern-defeating-quicksort-falseshao-jie](https://speakerdeck.com/po3rin/go1-dot-19decai-yong-sareta-pattern-defeating-quicksort-falseshao-jie ) 

- 演算法實作： [https://github.com/orlp/pdqsort](https://github.com/orlp/pdqsort ) 
- 作者說明影片： [https://www.youtube.com/watch?v=jz-PBiWwNjc](https://www.youtube.com/watch?v=jz-PBiWwNjc)
- [Golang 的排序演算法將換成 pdqsort，LLVM libc++ 換成 BlockQuicksort](https://blog.gslin.org/archives/2022/04/22/10673/golang-%E7%9A%84%E6%8E%92%E5%BA%8F%E6%BC%94%E7%AE%97%E6%B3%95%E5%B0%87%E6%8F%9B%E6%88%90-pdqsort%EF%BC%8Cllvm-libc-%E6%8F%9B%E6%88%90-blockquicksort/)

