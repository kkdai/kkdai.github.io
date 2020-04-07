---
layout: post
title: "[研討會心得] 2019/05/31 LINE 台灣 Warm-up meetup 活動後分享"
description: ""
category: 
- 研討會心得
tags: ["研討會心得", "BLE", "LINE_Things", "DevRel", "chatbots","LINE"]
---


# 前提

大家好，我是 LINE Taiwan 的 Technical Evangelist - Evan Lin。身為數位科技時代的各位，家庭中有經常多達十幾款不同的家電，而每個廠商都有自己的App。如果要能夠完全使用這些家電的功能的話，必須安裝至少十多個 App，其中有些廠商甚至推出兩個以上不同功能導向的 App， 這讓許多使用者感到相當困擾，而為了解決這種 App 過多的問題，LINE Things 應運而生！ 

就在五月中的時候，[LINE Things 自動通訊功能](https://engineering.linecorp.com/zh-hant/blog/introduction-line-things-automated-communication/#) 隨著最新版的 LINE (9.6) 的更新也開放給所有人。也讓開發者透過 LINE Things 能夠達到的事情也更多了。本活動特別請到日本團隊 LINE Things Leader - Takaku Hiroo 來到台灣與特地邀請的 maker 社群的開發代表們介紹新功能架構之外，也會透過簡單的範例帶領大家的來建置一個 LINE Things 的範例。最後也會開放社群開發者們提問與各種交流討論。

KKTIX 活動網頁:  [活動網址](https://linegroup.kktix.cc/events/20190531-linethings)

LINE Developer Meetup 開發者小聚系列活動將邀請 LINE 台灣工程師、開發者工具平台，定期分享內部開發技術，並安排優秀案例分享開發經驗，持續促進 LINE 技術平台開發與交流，歡迎對 LINE 技術平台有興趣的開發者參加。

## Introduction LINE Things architecture and how it works / LINE Things Leader - Takaku Hiroo  / LINE - Hazel Shen

#### [投影片](https://speakerdeck.com/line_developers/line-things-its-value-and-how-to-implement-it)

<script async class="speakerdeck-embed" data-id="28c1d1fd35e54f7c884bab4e6baf8eb9" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

首先一開始由來自日本的工程團隊代表 Takaku Hiroo 帶來對於 LINE Things 的架構解釋，並且跟開發者們解釋為什麼開發者們需要使用 LINE Things 。 

![](https://engineering.linecorp.com/wp-content/uploads/2019/03/hazel_final.png)

( 透過 LINE Things 開發的優點，圖片來自： [中文 Engineering Blog : 結合 LINE Things DIY一個智慧門鎖吧 ! ](https://engineering.linecorp.com/zh-hant/blog/line_things_smartlock/))



LINE Things 主要希望原本必須透過 iOS 與 Android 設計用來控制藍牙設備的 App 的一些廠商，透過 LINE Things 的使用可以達到「一份 code ，兩個平台」，也就是只要撰寫一次 LIFF 上面的藍牙控制相關程式碼，就可以讓使用者不論是透過 iOS 與 Android 的 LINE App 都可以達到只有 App 才能做到的事情。

由於發佈的方式也改成 LINE Chatbot 的方式，不僅能接觸到的使用者將會更多，也能夠更快速的更新軟體的版本。以往如果要開發一套新的設備，就必須要更新開發者的 App ，而且還得一次更新兩個版本。透過 LINE Things 就不需要那麼困擾，對於開發成本能夠降低之外，版本更新與部署的速度也會大大的提升。



![](https://engineering-org.line-apps.com/wp-content/uploads/2019/05/about-auto-communication-01.jpg)

( LINE Thing 自動通訊功能系統架構圖，圖片來自： [LINE Things 自動通訊功能已上線與使用介紹](https://engineering.linecorp.com/zh-hant/blog/introduction-line-things-automated-communication/))

而五月初隨著 LINE App 9.6 更新所上線的新功能 「自動通訊功能」 (Automatic Communication) 更是一個方便的功能，使用者再也不需要持續打開著 LIFF 的視窗就可以收到來自 BLE 設備的資料。詳細的圖片說明如下：

自動通訊可透過以下流程使LINE Bot和裝置進行連動。

1. 支援LINE Things 裝置的製作者經由 API，分別針對各個產品進行登錄通訊步驟（Scenario）。
2. LINE應用程式只要曾和支援 LINE Things 裝置連動過後，就會遵照事先登錄的 Scenario 進行通訊。
3. 若 Scenario 完成執行動作，LINE 應用程式就會透過 Webhook 將執行結果傳送給經由 Messagine API 完成裝置所綁定的 Channel 上。
4. 依據 Scenario 的執行結果，Channel 的 LINE Bot 就可以自由地回覆用戶。

講解完關於 LINE Things 的應用場景與解決的問題，並且也介紹最新的功能之後。 Hazel 就根據文章「[中文 Engineering Blog : 結合 LINE Things DIY一個智慧門鎖吧 ! ](https://engineering.linecorp.com/zh-hant/blog/line_things_smartlock/)」的內容逐步地帶領著相關的夥伴們解決相關的問題。



## Q&A / 交流時間 - Takaku Hiroo / Hazel Shen 

此次的來賓們皆為邀請制，主要分為 Maker 社群 (LASS) ，相關 IOT 技術廠商與一些技術合作夥伴。過程中收到許多來自不同領域夥伴的問題，將主要問題整理如下：



#### 藍牙裝置需要逐一配對，無法快速地切換？

**答:**   

- LINE Things 主要透過手機的藍牙配對成功後的設備開始控制，請注意手機上的藍牙裝置每一個皆有獨立的裝置 UUID 不會有重複的狀況出現。每個裝置需要逐一配對，而且每一個藍牙裝置被另外一個設備連線後就無法強制連線？ 這邊的行為並不是 LINE Things 特有的行為，是藍牙配對上的傳統行為。



### LINE Things 參考鏈結：

- [中文 Engineering Blog : LINE Things 自動通訊功能已上線與使用介紹](https://engineering.linecorp.com/zh-hant/blog/introduction-line-things-automated-communication/)
- [中文 Engineering Blog : 結合 LINE Things DIY一個智慧門鎖吧 ! ](https://engineering.linecorp.com/zh-hant/blog/line_things_smartlock/)
- [Engineering Blog: Trying out LINE’s IoT Platform through LINE Things Developer Trial](https://engineering.linecorp.com/en/blog/line-things-developer-trial/)
- [LINE Developer : LINE Things developer document](https://developers.line.biz/en/docs/line-things/)



## 活動小結

LINE Things 新功能  Automated Communication 上線，日本團隊特地到 LINE台灣辦公室希望能夠跟一些 Maker 社群與相關廠商見面。希望能夠透過功能介紹與展示 LINE Things 的最新功能獲得開發社群的意見回饋。 感謝旭多社群朋友的參加與意見討論，如果讀者為企業夥伴想要透過 LINE Things 機制有更多的合作歡迎透過[企業合作需求](https://partners.line.me/zh_TW) 送出貴公司的申請。




立即加入「LINE開發者官方社群」官方帳號，就能收到第一手Meetup活動，或與開發者計畫有關的最新消息的推播通知。▼

「LINE開發者官方社群」官方帳號ID：@line_tw_dev

## 關於「LINE開發社群計畫」

LINE今年年初在台灣啟動「LINE開發社群計畫」，將長期投入人力與資源在台灣舉辦對內對外、線上線下的開發者社群聚會、徵才日、開發者大會等，預計全年將舉辦30場以上的活動。歡迎讀者們能夠持續回來察看最新的狀況。詳情請看 [2019 年LINE 開發社群計畫活動時程表 (持續更新)](https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019-plan/)https://engineering.linecorp.com/zh-hant/blog/line-taiwan-developer-relations-2019-plan/)