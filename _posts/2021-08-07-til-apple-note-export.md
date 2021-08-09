---
layout: post
title: "[TIL] Apple notes (Apple 備忘錄) 的匯出策略"
description: ""
category: 
- TodayILearn
tags: ["KnowledgeManagement", "Tools"]
---



<img src="https://upload.wikimedia.org/wikipedia/commons/e/ed/Apple_Notes_%28iOS%29.png" width="400px">

## 前言:

Apple Notes (也就是 iOS / Mac OSX 上面的「備忘錄」) 是一個強大而且可以快速寫筆記的工具，支援簡單的 markdown 語法跟可以使用剪貼簿來貼圖片，其實對於許多使用者而言是相當的方便。 但是如果不小心上了 iCloud 就會全部將資料上了雲端，對於資安控管而言可就不妙了。 本篇文章記錄一些相關的搬遷方式，可以讓你將 Apple Notes 來匯出。



## 小心上雲端

首先要先注意，對於 Apple notes 而言本身並沒有分「雲端」與「本地端」的差別。只要你不小心將你的筆電 Apple Notes 連上了 iCloud 就再也無法順利存取到本地端。

```
有資安疑慮，絕對不要用 Apple Notes 上 iCloud!
有資安疑慮，絕對不要用 Apple Notes 上 iCloud!
有資安疑慮，絕對不要用 Apple Notes 上 iCloud!
```



## 不小心上了雲端，怎麼還原為「本地的 Apple Note」

這邊有點麻煩，其實簡單地答案是。**「辦不到」**。

可能做法有以下幾個：

- 備份為 PDF 然後刪除文章（無法匯入 Apple Notes)
- 切斷網路，然後砍掉 iCloud 資訊。
- 文章不多使用傳送來維持格式。



### 如果文章不多，可以用傳送。

為了維護格式的問題，可以透過傳送 (Air Drop )來傳送。

- 使用 iCloud 的手機（砍掉的資訊，會再刪除資料夾）
- 救回該筆記，然後將該筆記透過 Airdrop 來丟到 MacOSX。
- 如此重複，一篇一篇來丟。



## 即便你成功轉移到本地端 Apple Notes 筆電更換的時候，就必須要備份電腦

由於本地端的 Apple notes 存放地點在隱藏的 data 資料夾中，且格式也不好還原。 如果要換電腦得時候，可能就是要使用 Apple 筆電備份跟還原的功能。這樣也有可能會踩到資安控管的領域。



### 最近另外找到一個方式，就是換到 Bear notes

![](https://bear.app/static/images/video_placeholder.jpg)



[Bear notes App](https://bear.app/) 是一個很強大的筆記軟體，使用 Markdown  作為語法外，使用標籤與相互串連的方式。讓你整理資訊更加的方便。而且他提供一個方式可以將 Apple Notes 搬移到 Bear Notes。

[Migrate from Apple Notes](https://bear.app/faq/Import%20&%20export/Migrate%20from%20Apple%20Notes/) 這篇文章教導你透過一個小工具，可以把 Apple Notes 備份出來，然後透過 Bear Notes 強大的 Import 功能可以匯入到他的筆記之內。加上 Bear Notes 如果沒有付費，本身也是將資訊存在本地端。並且可以透過 [Backup and Restore Your Notes in Bear](https://bear.app/faq/Backup%20&%20Restore/) 來備份你的 Bear Notes 。

目前會朝向這個方向，繼續研究中。




## 相關文章：

- [Migrate from Apple Notes](https://bear.app/faq/Import%20&%20export/Migrate%20from%20Apple%20Notes/) 

-  [Backup and Restore Your Notes in Bear](https://bear.app/faq/Backup%20&%20Restore/)

- [Create local backup of notes in Notes.app on macOS](https://apple.stackexchange.com/questions/343221/create-local-backup-of-notes-in-notes-app-on-macos)
