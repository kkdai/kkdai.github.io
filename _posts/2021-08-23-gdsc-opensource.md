---
layout: post
title: "演講內容分享:「手把手教你建立自己的開源專案」"
description: ""
category: 
- 研討會心得
- DevRel
tags: ["研討會心得", "DevRel", "LINE", "TECHFRESH", "LINEDev", "OpenSource"]
---

![可能是顯示的文字是「 2021.08 手把手教導你如何建置開源專案 LINE Developer Relations Evan Lin LINE 」的圖像](https://scontent.ftpe8-2.fna.fbcdn.net/v/t39.30808-6/237290503_10222305088389882_6303611173398782921_n.jpg?_nc_cat=103&ccb=1-5&_nc_sid=730e14&_nc_ohc=zbmFUweBrE4AX_neJPW&tn=fE5B7NFKKVXKPnFB&_nc_ht=scontent.ftpe8-2.fna&oh=00e5c7a5f754629bf6189ace57171a44&oe=612A8844)

# 前言

大家好，我是 LINE Developer Relations 團隊的資深開發技術推廣工程師 - Evan Lin 。主要的工作項目就是平台技術推廣與技術品牌的建立與溝通。 這次很榮幸受到邀請幫 DSC (Developer Students Club) 開發者學生社群的暑期夏令營活動 (Summer BootCamp) 分享關於如何打造自己的開源專案的經驗分享。 

## 投影片

<script async class="speakerdeck-embed" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

# LINE 有參與開源專案嗎？

經常在參與許多開源聚會上，許多開發者朋友都不了解 LINE 有參與過相關的開源專案。但 LINE 其實也已經開源了超過 93 個專案，不僅有訊息平台的軟體開發工具套件，更有 LINE 開發內部專案過程中也在使用的套件：包括 [Armeria](https://github.com/line/armeria) 與 [Central Dogma](https://github.com/line/centraldogma) 等數個知名的開源專案，並且也開始經營相關的開源社群。大家可以參考一下 2019 年的 [COSCUP Keynote 分享 LINE 企業內部的開源流程](https://engineering.linecorp.com/zh-hant/blog/line-coscup-2019/)，並說明 LINE 企業文化鼓勵員工分享，更以開放的心胸接觸開源及參與開發者社群。

![img](https://engineering.linecorp.com/wp-content/uploads/2019/09/keynote_armeria-1024x768.jpg)

# 如何打造一個成功的開源專案

<script async class="speakerdeck-embed" data-slide="8" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

要打造一個知名的開源專案，建議各位同學們的步驟如下：

- 找到一個好點子 / Find a great idea 
- 良好的說明 / Well Documentation
- 完整的 CI/CD 流程 / Well Workflow
- 找到你的第一個貢獻者 / Find your first contributor 
- 宣傳! 宣傳! 宣傳! / Promote ! Promote ! 

這邊依序開始講解這些步驟，其中「找到一個好點子」放在最後（因為最難 :p )。

## 良好的說明 / Well Documentation

<script async class="speakerdeck-embed" data-slide="10" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

首先先分享給各位（也是最容易被開發者們忽略的）就是好的文件。（也許可能是開發者們刻意不想寫的 :p ) 。 對於軟體開發來說，好的文件相當的重要。 而開源專案最重要的就是 `README.md` 這個檔案往往會出現在 github 的專案頁面。透過上面的範例，這邊有一幾個重點希望同學們能注意到：

- **足夠多的 Badge （徽章）：**
  - 徽章本身是講解相關的狀況（ build 成功，有說明文件...)，此外也可以比較漂亮啦。
- **專案說明**： 
  - 一句短短話，讓路過的人知道你專案的摘要。
- **如何安裝 (install) /引用 (include)**: 
  - 這個往往是許多初期開源專案開發者遺忘的。你需要讓路過的人知道如何安裝，如何能夠正確 include 。這樣想要使用的人，不會第一部卡在環境設定上的相關問題。  比如說有一些 Python 相關的專案，在許多套件的相依性處理上，沒有寫清楚的話。往往之後看到的人都無法正確使用。自然而然就不會使用。
- **如何貢獻 / How to contribute :**
  - 這也是很重要的部分，包括了說明白開源專案的授權方式。 （可以參考 [自由及開放原始碼軟體授權條款比較](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%94%B1%E5%8F%8A%E9%96%8B%E6%94%BE%E5%8E%9F%E5%A7%8B%E7%A2%BC%E8%BB%9F%E9%AB%94%E8%A8%B1%E5%8F%AF%E8%AD%89%E6%AF%94%E8%BC%83) ) 還有就是可以告訴想要貢獻的人，你希望他們能跑過哪一些基本的 unit testing 。越多的說明就可以讓你的貢獻者越安心。

### 參考資料：

-  [自由及開放原始碼軟體授權條款比較](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%94%B1%E5%8F%8A%E9%96%8B%E6%94%BE%E5%8E%9F%E5%A7%8B%E7%A2%BC%E8%BB%9F%E9%AB%94%E8%A8%B1%E5%8F%AF%E8%AD%89%E6%AF%94%E8%BC%83) 
- 徽章列表 [badges](https://github.com/badges)/[shields](https://github.com/badges/shields)
- [How to Write a Good README File for Your GitHub Project](https://www.freecodecamp.org/news/how-to-write-a-good-readme-file/)

## 完整的 CI/CD 流程 / Well Workflow

<script async class="speakerdeck-embed" data-slide="11" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

這邊指的是相關的 Github Action ， Github Action 可以幫助開源專案以下幾個部分：

- 如果有 Pull Request 發進來，如果有好的 Github Action 設置，可以檢查程式碼有無編譯錯誤( Build Failed) 。
  - [actions/setup-go: Set up your GitHub Actions](https://github.com/actions/setup-go)
  - [Building and testing Python - GitHub Docs](https://docs.github.com/en/actions/guides/building-and-testing-python)
- 編譯好的執行檔，讓人可以直接下載發布版本 (Release Version)。透過好的設置，甚至想使用的人不需要設定環境，可以直接下載可以執行的版本，就可以直接使用。這邊可以參考:
  - [GoReleaser Action - GitHub](https://github.com/goreleaser/goreleaser-action)
  - [How to release Python package from GitHub Actions](https://blog.chezo.uno/how-to-release-python-package-from-github-actions-d5a1d8edba6e)

### 參考資料：

- [什麼是 PR: Pull Request](https://yingchencheng.medium.com/github-%E4%B8%8A%E5%B8%B8%E5%B8%B8%E5%87%BA%E7%8F%BE%E7%9A%84%E7%B8%AE%E5%AF%AB-b7aa396971a1#:~:text=PR%20(Pull%20Request)&text=%E4%BF%AE%E6%94%B9%E5%AE%8C%E6%88%90%E5%BE%8C%EF%BC%8C%E5%85%88%E6%8E%A8,%E7%9C%8B%E4%B8%80%E4%B8%8B%E4%BD%A0%E7%9A%84%E4%BF%AE%E6%94%B9%E3%80%82&text=%E4%B8%8A%E9%9D%A2%E7%9A%84%E6%B5%81%E7%A8%8B%E4%B8%AD%EF%BC%8C%E3%80%8C%E7%99%BC,%E5%8B%95%E4%BD%9C%E5%B0%B1%E5%8F%AB%E5%81%9APull%20Request%E3%80%82) 

## 找到你的第一個貢獻者 / Find your first contributor 

<script async class="speakerdeck-embed" data-slide="12" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

當你為了你的開源專案準備好了文件，也準備好所有的流程後。接下來就是開門來歡迎第一位一起開發的人員了。（這邊往往需要很多的時間）。到底要如何找到你的第一位貢獻者呢？ 

這時候第一步建議你可以先為了自己的專案寫下幾個讓想要貢獻的夥伴們可以上手的部分。這個在開源社群被稱為是 「Good First Issue」。 透過這些比較容易上手的問題：

- 可能是文件修改（中文化，日文化等等）
- 可能是加參數。
- 相關文件補充需求。

這些可以讓想要幫忙的人有一個好的開始，也是可以吸引到更多願意幫忙的人的方式。

### 參考資料：

- [Good First Issue: Issues for your first open-source contribution](https://goodfirstissue.dev/)
- [Encouraging helpful contributions to your project with labels](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/encouraging-helpful-contributions-to-your-project-with-labels#:~:text=On GitHub%2C navigate to the,start typing good first issue .)

## 宣傳! 宣傳! 宣傳! / Promote ! Promote ! 

<script async class="speakerdeck-embed" data-slide="13" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

做好了相關的說明後，建議要經常去推廣你的專案。畢竟你需要透過不段的推廣，你也才知道哪些東西是大家有興趣的。推廣的方式可以有以下幾個方式：

-  準備一些說明文章，透過文章的說明來解釋這個 Github Repository 的主要功能。雖然在 README 會提到，但是透過文章的敘事方式往往可以讓更多的人願意去了解你的專案本質，體會專案主要解決的痛點。
- 分享！ 就是不斷的第透過線上分享，線上演講去分享。這也是最直接的方式可以讓你接觸到你的潛在用戶。相當推薦可以在「[開源者年會](https://en.wikipedia.org/wiki/COSCUP)」去分享你的專案，每一次的分享就會直接有許多星星（Github Like ) 的進帳。

## 找到一個好點子 / Find a great idea 

<script async class="speakerdeck-embed" data-slide="16" data-id="7c88264dc5594cc2846386c275f1989a" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

最後，也就是最難的環節。 經常有同學與朋友問我，你怎麼有那麼多的點子可以準備那麼多的開源專案？（筆者的開源專案有接近 200 個 repositories)。這邊想要跟各位分享的是，建議方式有以下兩個：

- 透過一些小工具 Trello 或是其他記事工具，把你想要打造的工具先記錄下來。 有空的時候把它拿出來開始寫。
- 看到好的專案，先試著 Fork出來開始學習裡面相關內容。可以透過你喜歡的語言，或是練習寫一個更精簡的版本出來（也就是功能比較少的版本）。

透過這兩個方式，主要分享給同學們的是：

- 不要因為靈感而卡住，開源專案的重點就是努力寫，拼命寫。覺得有趣就可以寫。
- 很多時候，學習其他人偉大的專案。往往是開啟自己專案的好契機。

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

