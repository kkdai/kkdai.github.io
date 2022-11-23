---
layout: post
title: "演講內容分享:「如何建立自己的開源專案」"
description: ""
category: 
- 研討會心得
- DevRel
tags: ["研討會心得", "DevRel", "LINE", "TECHFRESH", "LINEDev", "OpenSource"]
---

Img

# 前言

大家好，我是 LINE Developer Relations 團隊的資深開發部門的 - Evan Lin 。主要的工作項目就是平台技術推廣與技術品牌的建立與溝通。 這次很榮幸受到邀請幫陽明交大的 DSC (Developer Students Club) 開發者學生社群的來分享關於如何打造自己的開源專案的經驗。 

## 投影片

<script async class="speakerdeck-embed" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

內容有一些部分歡迎參考這篇文章[LINE 開發社群計畫: 「手把手教你建立自己的開源專案」](https://engineering.linecorp.com/zh-hant/blog/gdsc-opensource/)，以下將特別列出

# 開源專案與學生實習工作的關係

最近經常看到同學們在討論，到底從事開源專案或是寫一些自己的 side project 跟工作有沒有關係？ 這邊可以分享關於 LINE 的實習工作機會 LINE TECH FRESH  就很在意你的開源專案，因為從開源專案可以看出以下一些重點：

- 你是否有良好的文件習慣（端看 README.md) 
- 專案是否有良好的 CI/CD 流程，也代表了你是否懂一些基本的流程。
- 看相關專案的程式碼，可以了解你對於 Git 了解的程度。
- 除了這些之外，好的開源專案代表你也有良好的 Pull Request 的訓練與習慣。對於多國協作上，你也更容易能適應相關團隊合作的方式。

# 關於 LINE 學生實習機會: LINE TECH FRESH 介紹


LINE 台灣工程團隊每年透過 [LINE TECH FRESH – 技術新星人才計劃](https://career.linecorp.com/linecorp/career/detail/20000111/704/5570?classId=&locationCd=TW&page=)，招募資訊科技相關科系，或對此領域有所涉略的大學生 / 研究生加入 LINE 團隊進行長期實習 (一年期)，讓同學們能在國際級科技公司中觀摩學習。LINE TECH FRESH 由經驗豐富的技術專案經理帶領團隊，接觸多元化的專案與產品開發，學習業界實際的軟體專案分工，並體驗跨國團隊合作。往年工作內容包含 server、web、mobile app、chatbot、IoT、data、DevOps 等領域，並透過實習熟悉 LINE 平台系統、SDK、API 等。值得一提的是，LINE TECH FRESH 是有給薪的實習機會，對於軟體開發有熱情、有想法的同學們，千萬別錯過這個揮灑創意與衝勁的機會！

更多關於 LINE TECH FRESH 介紹文章有:

- [TECH FRESH 實習的一年間，除了開發還有什麼內部活動呢？](https://engineering.linecorp.com/zh-hant/blog/line-tech-fresh-2020-graduate/)

- [【訪談】TECH FRESH 工作老實說 – 後續花絮與相關資訊整理](https://engineering.linecorp.com/zh-hant/blog/what-is-tech-fresh-interview/)

- [Life in LINE – 直擊 TECH FRESH 實習內容！](https://engineering.linecorp.com/zh-hant/blog/life-in-line-tech-fresh-sharing/)

- [TECHPULSE 2020 青春主場 – TECH FRESH 議程與攤位介紹](

# 同學們的相關詢問：


## 1. 如何讓自己的 Github 容易被找到?

###  A:

- 試著多寫一點文章，每一篇文章都是 SEO 好的入口。可以讓更多的人看見你的專案。
- 永遠別忘了，要將自己得開源專案當成產品一樣宣傳。可以多參與一些演講來分享。



## 2. Github 專案建議以 Quality 為主，還是 Quantaty 最主？

### A:

- 建議想到就寫，因為你不會知道哪個專案會紅。
- 我也有許多專案，因為機運就超過了 1K 的 Star 。



## 3. 會不會擔心自己的專案被人家抄走?

### A:

- 不會，開源專案不需要擔心你的專案被抄走。與其擔心被抄走，你應該更擔心你的專案一顆 Star 都沒有。
- 真正擔心的部分，建議都先寫論，先寫專案。然後才開源。



## 4. 哪一些 Github 是企業比較在意的？

### A:

- 如果以 LINE 而言，如果你有 LINE Bot 的開源專案，我們除了可以知道你已經對於公司相關聊天機器人瞭解之外。也能了解你對於錯誤控管的方式（也就是對於使用者隨意輸入文字的處理），可以透過這些方是來了解每一個開發者細心的程度。
- 跟前面呼應類似的話題，主要是看你每一個專案處理的細緻程度。有沒有文件化，有沒有良好的流程。

## 5. 是不是一定要學會 Git 指令才能開始做開源專案？

### A:

- 不需要，許多同學也來問我是否需要買一本 Git 教學手冊才開始做開源專案？
- 其實不需要，建議先開始建立專案。往往許多基本指令，可以透過 VSCode 等等相關軟體都可以快速協助你處理。
- 真正等你需要更底層的指令，你就會去查詢 `git pull -rebase` 等等相關指令。



## 6. 還有問題該如何問？

### A:

- 如果你還沒開啟你的 Github 帳號，而且你有許多疑問。可以考慮透過 Github 來詢問我。
- 任何開源問題都歡迎： [https://github.com/kkdai/AMA](https://github.com/kkdai/AMA) ，你也可以學習如何開啟一個 issue。
- 小絕竅： Github Issue 也算是一種 Contribution 喔！


# 關於 LINE  開發者官方社群


立即加入「LINE開發者官方社群」官方帳號，就能收到第一手Meetup活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE開發者官方社群」官方帳號ID：[@line_tw_dev](https://lin.ee/s5RsZHo)

![](http://www.evanlin.com/images/2020/line-tw-dev-qr.png)

## 關於「LINE開發社群計畫」

LINE今年年初在台灣啟動「LINE開發社群計畫」，將長期投入人力與資源在台灣舉辦對內對外、線上線下的開發者社群聚會、徵才日、開發者大會等，已經舉辦30場以上的活動。歡迎讀者們能夠持續回來察看最新的狀況。詳情請看:

- [2019 年LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019-plan/)
- [LINE Taiwan Developer Relations 2019 回顧與 2019 開發社群計畫報告](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019/)
- [2020 年LINE 開發社群計畫活動時程表](https://engineering.linecorp.com/zh-hant/blog/2020-line-tw-devrel/)

