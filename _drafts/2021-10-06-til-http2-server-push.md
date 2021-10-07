---
layout: post
title: "[TIL][學習] 學習 HTTP2 Server Push - To push, or not to push?! - The future of HTTP/2 server push - Patrick Hamann"
description: ""
category: 
- TodayILearn
- 機器學習
tags: ["TIL", " HTTP/2"]
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/cznVISavm-k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



# 前提




# Critical Request

- 顯示出使用者最在意的資料。

# Original HTTPS

![image-20211007144201246](../images/2021/image-20211007144201246.png)



- CSS could not be pared incrementally 
- DOM need to be combination to final result

## Critical Hidden Resource

![image-20211007144209222](../images/2021/image-20211007144209222.png)

- 透過三行 Header  可以告訴 browser 哪些資源最重要。

### 在 Apply 之前

![image-20211007144216401](../images/2021/image-20211007144216401.png)

相關的 Font 是最後才下載

### Apply 之後

![image-20211007144225091](../images/2021/image-20211007144225091.png)

Font 變成是同步下載，可以讓相關資料顯示的正確且美觀。

## 其他公司透過 Pre-load FONT 達成的結果



![image-20211007144414420](../images/2021/image-20211007144414420.png)

Shopify 透過 Pre-load Font 達成 50% 的呈現速度（約為 1.2 秒)

# Server Push 







# Reference

- 
