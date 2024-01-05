---
layout: post
title: "[Golang][LINE Bot SDK] 如何更新 Golang LINE Bot SDK v8 OpenAPI(Swagger)"
description: ""
category: 
- Golang
tags: ["LINEBot", "Golang", "OpenAPI"]

---

![image-20240105183407936](../images/2022/image-20240105183407936.png)

# 前情提要：

2023 年 [LINE Bot SDK 積極推動 OpenAPI](https://github.com/line/line-openapi) (a.k.a. swagger) 的標準介面。透過與 OpenAPI 的整合， LINE Bot SDK 有了許多好處（文章後半段補充）。本篇文章將稍微解釋一下，OpenAPI 導入後的優點，並且帶著大家一起來將有使用 [LINE Bot Go SDK](https://github.com/line/line-bot-sdk-go) v7 版本的升級到 v8 的版本。



## 支援 OpenAPI 的好處

 [LINE Bot Go SDK](https://github.com/line/line-bot-sdk-go) 近期也在 [2023/11 月](https://github.com/line/line-bot-sdk-go/releases/tag/v8.0.0)也將版號更新到了 v8，並且正式支援 [OpenAPI](https://github.com/line/line-openapi/)。 那麼簡單的來說，一個 SDK 套件支援 OpenAPI 有哪些好處呢？ 

#### 1. 標準化的 API 設計

- 許多 API 呼叫的方式，變得更加的直觀。也比較容易了解。後面也會稍微提到。



#### 2. 程式碼自動生成

<img src="../images/2022/image-20240105200853423.png" alt="image-20240105200853423" style="zoom:25%;" />

- 以往要開發新的 API Entry 的時候，都要透過發送 issue -> 發送 PR -> 審核 -> 發布後才能發送到開發者手中。
- 但是導入後， LINE Bot SDK 使用的方式，是透過 Github Action 來自動去將最新的 [LINE OPENAPI Repo](https://github.com/line/line-openapi)抓下來後，根據新的變動產生相關的對應程式碼。 (參考上圖)

更多好處：

- **自動化文件生成:**有許多相關文件可以幫忙自動化產生 OpenAPI 的文件。

- **客戶端與服務端驗證:**透過 OpenAPI 的導入可以達成自動化測試與 Consumer-Driven Contract 的驗證。
- **API 生態系統工具的整合：**


## 使用 LINE Bot SDK 套件的開發者有哪些好處？

- 以往新的 API ([refer news](https://developers.line.biz/en/news/2023/05/29/use-audiences-created-with-messaging-api-for-step-messages/)) 都可能要等上一兩個禮拜才能有新的 SDK 可以用，現在更新的頻率將會更快。
- 

 

# 參考資料：

- [LINE OPENAPI Repo](https://github.com/line/line-openapi)

- 
