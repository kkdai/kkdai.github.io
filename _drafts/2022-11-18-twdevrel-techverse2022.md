---
layout: post
title: "[TW_DevRel] TECH-Verse 2022 有趣的議程分享 - 第一天"
description: ""
category: 
- 研討會心得
tags: ["研討會心得", "LINE"]
---

![image-20221118150105191](../images/2021/image-20221118150105191.png)

# 前言

11/17 ~ 11/18 兩天在線上舉辦的 Z Holding 開發者大會 TECH-Verse 即將展開。包括了 LINE Corp, Yahoo! Japan 等8家公司的線上開發者大會，共有 90 線上議程：

所有相關議程類別條列如下：

- Day 1 - 11.17 (11:00 ~ 18:00)
  - Data / AI
  - Security
  - Infrastructure
  - Blockchain
- Day 2 - 11.18 (10:00 ~ 18:00)
  - Server Side
  - UX / Design
  - Mobile App
  - Web Front-end
  - Process & Environment

相關活動網址： [https://tech-verse.me/en](https://tech-verse.me/en)

相關的議程，結束後馬上都有投影片的公開。相關的口譯完成後的影片，將統一在 11/25 之後陸續公開。

# 第一天有趣的議程分享：

![img](https://github.com/vdaas/vald/raw/main/assets/image/readme.svg)

## Vald: ANN search engine by Golang**Vald: OSS ANN Nearest Neighbor Dense Vector Search Engine

Valid: https://github.com/vdaas/vald

這是一個 scalable 分散式的 ANN (approximate nearest neighbor) vector search engine 這是一套可以很迅速在 K8S 上面部署的的工具。 在 Yahoo! JP 裡面更有用來使用:

- 相似產品圖片搜尋
- 透過產品名稱來產生相關 Tag
- 相似 source code search (這也行！)

想知道如何使用，可以參考

- [投影片](https://speakerdeck.com/techverse_2022/vald-oss-ann-nearest-neighbor-dense-vector-search-engine-introduction-and-case-studies)。
- 議程： https://tech-verse.me/en/sessions/172

## Our Automation Tool for Migrating 1,800 MySQL Instances in Only Six Months

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/52b80ea6286f4fa9ad587699c3324731" title="Our Automation Tool for Migrating 1,800 MySQL Instances in Only Six Months" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 560px; height: 314px;" data-ratio="1.78343949044586"></iframe>

請問各位有沒有進行過 MySQL 的升級？ LINE 由於事業擴展的原因，整個企業中的 MySQL Instance 超過 6000 個。為了要提升 MySQL 5.6 (已經在 2021/02 停止維護)的版本，將會是一個無法計算的成本。 

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/52b80ea6286f4fa9ad587699c3324731?slide=15" title="Our Automation Tool for Migrating 1,800 MySQL Instances in Only Six Months" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 560px; height: 314px;" data-ratio="1.78343949044586"></iframe>

一般來說，MySQL 更新流程可以分成:

- Create New MySQL Instance
- Add User ACL
- Set MySQL Variable
- Export/Import Data
- Start Replication
- Inspect Query Performace
- Switch to new MySQL. (In midnight?)
- Shutdown old MySQL

這樣的工作，其實是相當耗時的。 有可能做完就..... 

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/52b80ea6286f4fa9ad587699c3324731?slide=17" title="Our Automation Tool for Migrating 1,800 MySQL Instances in Only Six Months" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 560px; height: 314px;" data-ratio="1.78343949044586"></iframe>

(這個講者真的很風趣誒)

所以 LINE 的 DBA 團隊，開發出一個工具 MUH: (MySQL Update Helper)

如何透過 MUH 可以幫你把 MySQL 升級又不會有資安考量。可以參考這篇投影片講解原理：

- 投影片 https://speakerdeck.com/techverse_2022/our-automation-tool-for-migrating-1800-mysql-instances-in-only-six-months
- 議程： https://tech-verse.me/en/sessions/61

# 活動小結

立即加入「LINE 開發者官方社群」官方帳號，就能收到第一手 Meetup 活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE 開發者官方社群」官方帳號 ID：@line_tw_dev
![img](https://www.evanlin.com/images/2020/line-tw-dev-qr.png)

# 關於「LINE 開發社群計畫」

LINE 2019 年初在台灣啟動「LINE 開發社群計畫」，將長期投入人力與資源在台灣舉辦對內對外、線上線下的開發者社群聚會、徵才日、開發者大會等，已經舉辦 30 場以上的活動。歡迎讀者們能夠持續回來察看最新的狀況。詳情請看:

- [2019 年 LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019-plan/)
- [LINE Taiwan Developer Relations 2019 回顧與 2019 開發社群計畫報告](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019/)
- [2020 年 LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/2020-line-tw-devrel/)
- [2021 年 LINE 開發社群計畫活動時程表 (持續更新)](https://engineering.linecorp.com/zh-hant/blog/2021-line-tw-devrel/)