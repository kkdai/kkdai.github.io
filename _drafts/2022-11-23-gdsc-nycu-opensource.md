---
layout: post
title: "演講內容分享:「如何建立自己的開源專案」"
description: ""
category: 
- 研討會心得
- DevRel
tags: ["研討會心得", "DevRel", "LINE", "TECHFRESH", "LINEDev", "OpenSource"]
---

![img](https://lh3.googleusercontent.com/pw/AL9nZEVlzhyxWZjbu88nkA87feJoVYHRrv5OuAwLTH7hrFzuFxISNwaBh99z8SE6Coc6wb5m-VfuFvfL27a40mbob42MYIAVYgDB3KOjeSbSNME4WO6ArvE4eyLxttuIg7QUSb4CVqZjyRofVu_vr3ZHtFz9Qw=w2362-h1578-no?authuser=0)

# 前言

大家好，我是 LINE Developer Relations 團隊的資深開發部門的 - Evan Lin 。主要的工作項目就是平台技術推廣與技術品牌的建立與溝通。 這次很榮幸受到邀請幫陽明交大的 DSC (Developer Students Club) 開發者學生社群的來分享關於如何打造自己的開源專案的經驗這次的同學相當的踴躍，很感謝同學在下雨的晚上願意聽我講解如何參與開源專案的開發。



## 投影片

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/93aa38e46a4b47d199a7db57918f1df1" title="如何建立自己的開源專案" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 560px; height: 314px;" data-ratio="1.78343949044586"></iframe>

內容有一些部分歡迎參考這篇文章[LINE 開發社群計畫: 「手把手教你建立自己的開源專案」](https://engineering.linecorp.com/zh-hant/blog/gdsc-opensource/)，裡面內容有包括：

- 找到一個好點子 / Find a great idea
- 良好的說明 / Well Documentation
- 完整的 CI/CD 流程 / Well Workflow
- 找到你的第一個貢獻者 / Find your first contributor
- 宣傳! 宣傳! 宣傳! / Promote ! Promote !

以下將特別列出本次演講分享新內容:

# 開源專案對於工作的關係有沒有用?

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/93aa38e46a4b47d199a7db57918f1df1?slide=32" title="如何建立自己的開源專案" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 560px; height: 314px;" data-ratio="1.78343949044586"></iframe>

最近經常看到同學們在討論，到底從事開源專案或是寫一些自己的 side project 跟工作有沒有關係？ 這邊可以分享關於 LINE 的實習工作機會 LINE TECH FRESH  就很在意你的開源專案，因為從開源專案可以看出以下一些重點：

- 你是否有良好的文件習慣（這看你專案的  README.md 就能展現出來) 
- 專案是否有良好的 CI/CD 流程，也代表了你是否懂一些基本的流程。
- 看相關專案的程式碼，可以了解你對於 Git 了解的程度。
- 除了這些之外，好的開源專案代表你也有良好的 Pull Request 的訓練與習慣。對於多國協作上，你也更容易能適應相關團隊合作的方式。



# 刷題目到底重不重要？

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/93aa38e46a4b47d199a7db57918f1df1?slide=31" title="如何建立自己的開源專案" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 560px; height: 314px;" data-ratio="1.78343949044586"></iframe>

YES, YES and YES。

如果你是軟體研發相關工作，這個絕對重要。就像你考上大學聯考一樣，如果進入這些國立大學的路途只有考試。即便你有各種才藝，考試的分數依舊是你唯一證明的依據之一。

刷題目對研發工程師來說，最重要莫過：

- **對於問題的分析：**能夠精準的閱讀題目，了解真正的問題。
- **對於資料結構的熟悉：**熟悉資料結構與它能解決的問題。
- **演算法：** 熟悉每一種演算法與它可能的時間複雜度，才能針對題目來使用。

這些都是身為程式設計師重要的技能，所以我認為這個只是基本。 而你面對的是當所有人都是刷題刷過了，而你有開源的經驗。那麼對面試官來說，你就有相當的優勢。（重要是不需要花時間教你）。

# 關於 LINE 學生實習機會: LINE TECH FRESH 介紹


LINE 台灣工程團隊每年透過 [LINE TECH FRESH – 技術新星人才計劃](https://career.linecorp.com/linecorp/career/detail/20000111/704/5570?classId=&locationCd=TW&page=)，招募資訊科技相關科系，或對此領域有所涉略的大學生 / 研究生加入 LINE 團隊進行長期實習 (一年期)，讓同學們能在國際級科技公司中觀摩學習。LINE TECH FRESH 由經驗豐富的技術專案經理帶領團隊，接觸多元化的專案與產品開發，學習業界實際的軟體專案分工，並體驗跨國團隊合作。往年工作內容包含 server、web、mobile app、chatbot、IoT、data、DevOps 等領域，並透過實習熟悉 LINE 平台系統、SDK、API 等。值得一提的是，LINE TECH FRESH 是有給薪的實習機會，對於軟體開發有熱情、有想法的同學們，千萬別錯過這個揮灑創意與衝勁的機會！

更多關於 LINE TECH FRESH 介紹文章有:

- [TECH FRESH 實習的一年間，除了開發還有什麼內部活動呢？](https://engineering.linecorp.com/zh-hant/blog/line-tech-fresh-2020-graduate/)

- [【訪談】TECH FRESH 工作老實說 – 後續花絮與相關資訊整理](https://engineering.linecorp.com/zh-hant/blog/what-is-tech-fresh-interview/)

- [Life in LINE – 直擊 TECH FRESH 實習內容！](https://engineering.linecorp.com/zh-hant/blog/life-in-line-tech-fresh-sharing/)

- [TECHPULSE 2020 青春主場 – TECH FRESH 議程與攤位介紹](https://engineering.linecorp.com/zh-hant/blog/techpulse-2020-tech-fresh-session/)

- [LINE TECH FRESH 2022 面試分享](https://engineering.linecorp.com/zh-hant/blog/line-tech-fresh-interview-2022/)

- [【LINE TECH FRESH】2021 屆畢業囉！這一年來又有什麼新鮮事呢？](https://engineering.linecorp.com/zh-hant/blog/line-tech-fresh-2021-gradute/)

- [Data Dev Team 實習生在做什麼？LINE TECH FRESH 生活心得分享](https://engineering.linecorp.com/zh-hant/blog/data-dev-team-tech-fresh-life-2/)

- [資料工程團隊 TECH FRESH 實習生活分享](https://engineering.linecorp.com/zh-hant/blog/data-dev-team-tech-fresh-life-1/)

- [LINE Dev Meetup 16 TECH FRESH 校園招募現場活動分享](https://engineering.linecorp.com/zh-hant/blog/line-dev-meetup-16/)


# 同學們的相關詢問：


## 1. 

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

