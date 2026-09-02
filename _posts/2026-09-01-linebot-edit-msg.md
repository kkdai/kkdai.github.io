---
layout: post
title: "[AI 實戰] 當訊息可以反悔：用 Edit 與 Unsend Webhook 打造會「變形」的 LINE 團購 Bot"
description: "當訊息可以反悔：用 Edit 與 Unsend Webhook 打造會「變形」的 LINE 團購 Bot"
category:
- LINEBot
tags: ["LINEBot", "Messaging API"]
---



<iframe width="485" height="862" src="https://www.youtube.com/embed/ldjbkUSKZCs" title="編輯訊息 LINE Bot 測試影片 - 百變店長・單單" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# 當訊息可以反悔：用 Edit 與 Unsend Webhook 打造會「變形」的 LINE 團購 Bot

作者：Evan Lin，LINE Taiwan Developer Relations Team Lead

2026 年 8 月 20 日，LINE 宣布在 LINE Labs 開放「編輯訊息」功能免費試用。

對一般使用者來說，這是一個很直覺的功能：訊息送出後才發現打錯字、日期寫錯，或語氣想再調整，不必再經過「收回、重打、重新送出」的流程，直接編輯原本的文字訊息就可以了。

但我看到這個功能時，第一個想到的是另一件事：

> 如果使用者改掉一則已經被 LINE Bot 處理過的訊息，Bot 知道嗎？

答案是：知道。LINE Messaging API 提供了 Edit event；而當使用者收回訊息時，也有 Unsend event 可以接收。

這篇文章想跟大家分享，我如何把這兩種 webhook event 做成一個有故事的 LINE Bot Demo，以及實作時幾個容易忽略、卻非常重要的細節。

完整範例程式碼放在 GitHub：

<https://github.com/kkdai/linebot-edit-unsend>

## 先從 LINE Labs 的「編輯訊息」開始

目前在 LINE Labs 試用編輯訊息，需要先符合以下條件：

1. 手機版 LINE 更新至 26.12.0（含）以上版本。
2. 進入「主頁 → 設定 → LINE Labs」。
3. 開啟「訊息編輯」。

在一般一對一與群組聊天室中，文字訊息送出後 15 分鐘內可以編輯；Keep 筆記則是 6 天內。照片、影片、語音、檔案與貼圖目前不能編輯。修改後，聊天室會顯示「已編輯」，但不提供舊版本查看或還原。

這裡還有一個跟 Bot 開發直接相關的限制：目前與 LINE 官方帳號的一對一聊天室不支援編輯訊息。因此，要測試 Messaging API 的 Edit event，必須把 Bot 加入群組聊天室。

功能與啟用方式可以參考 [LINE 台灣新聞室公告](https://www.linecorp.com/tw/pr/news/2026/0820/)。

## 從 UI 功能想到 Bot 的資料一致性

過去 Bot 收到文字訊息後，常見的處理流程是：

1. 接收 message webhook。
2. 解析文字。
3. 寫入資料庫或觸發後續流程。
4. 回覆處理結果。

如果訊息送出後不能修改，這個模型很單純。但當訊息可以被編輯，原本的文字便不一定是使用者最後的意圖。

例如使用者送出：

```text
珍珠奶茶 / 半糖 / 少冰 / 1
```

Bot 已經把它記錄成一杯 60 元的飲料。幾秒後，使用者直接編輯原訊息：

```text
珍珠奶茶 / 微糖 / 去冰 / 2
```

如果 Bot 沒有處理 Edit event，聊天室看到的是兩杯，但後端仍然記成一杯。畫面上的世界和 Bot 裡的世界就分開了。

這就是 Edit event 最實際的價值：它不只是通知「文字改過了」，而是讓服務有機會同步使用者的最新意圖。

## 團購變形記：讓 API Demo 有一個故事

為了同時展示 edit 與 unsend，我把 Bot 設定成「百變店長・單單」，負責拯救下午三點陷入低電量的辦公室。

Demo 流程如下：

1. 團主輸入 `開團 五十嵐 15:20 截單`。
2. Alice 輸入 `珍珠奶茶 / 半糖 / 少冰 / 1`。
3. Alice 編輯同一則訊息，改成 `珍珠奶茶 / 微糖 / 去冰 / 2`。
4. Bot 收到 Edit event，更新杯數、甜度、冰量及總金額。
5. Bob 下單後收回訊息。
6. Bot 收到 Unsend event，刪除 Bob 的訂單並重新計算。
7. 輸入 `目前訂單`，以 Flex Message 查看最新總結。

這個場景很適合 Demo，是因為大家都知道團購最常發生的兩件事：下單後改口，以及下單後突然不喝了。

更重要的是，觀眾可以直接從杯數和金額看出 webhook 是否真的影響了後端狀態，而不只是 Bot 回覆一句「收到編輯」。

## Edit event 的結構

Edit event 的 `type` 是 `messageEdited`，其中會帶著編輯後的文字、timestamp、reply token 與 message ID：

```json
{
  "type": "messageEdited",
  "replyToken": "950e63e8f46542ab89f645b4c2a1180a",
  "message": {
    "type": "text",
    "id": "610830548529053697",
    "text": "珍珠奶茶 / 微糖 / 去冰 / 2"
  },
  "timestamp": 1776914799524,
  "source": {
    "type": "group",
    "groupId": "Ca56f94637c...",
    "userId": "U4af4980629..."
  }
}
```

最關鍵的設計是：編輯事件中的 `message.id`，與原本 message event 的 message ID 相同。

因此，我們可以直接把 message ID 當成訂單 ID：

```go
case webhook.MessageEvent:
    h.handleMessage(e)
case webhook.MessageEditedEvent:
    h.handleEdit(e)
case webhook.UnsendEvent:
    h.handleUnsend(e)
```

收到原始 message event 時新增訂單；收到 `MessageEditedEvent` 時，用相同 message ID 找出訂單並覆寫內容。

另外，Edit event 有自己的 reply token，而且與原始 message event 的 reply token 不同，因此 Bot 可以直接針對這次編輯回覆「訂單變形成功」。

如果你的 Bot 同時使用 Mark as Read API，也要注意 Edit event 不會帶 `markAsReadToken`，不能把一般 message event 的處理流程原封不動套過來。這類欄位差異很適合在升級 SDK 後，搭配 webhook fixture 做一次完整測試。

完整欄位請參考 [Messaging API：Edit event](https://developers.line.biz/en/reference/messaging-api/#edit-event)。

## 第一個陷阱：Edit event 可能不按順序抵達

這是這次實作中最容易被忽略的地方。

使用者可能連續編輯同一則訊息，而多個 `messageEdited` webhook 不保證按照編輯順序抵達。官方文件建議以最大的 timestamp 代表最新狀態。

因此不能單純採用「最後收到的 webhook」，而應該採用「timestamp 最新的 webhook」：

```go
if timestamp <= current.UpdatedAt {
    return summarize(buy), errStaleEdit
}

current.UpdatedAt = timestamp
buy.Orders[messageID] = current
```

假設第二次編輯先抵達、第一次編輯稍後才抵達，這個判斷可以避免舊內容蓋掉新內容。

同時，LINE Platform 也可能重新傳送 webhook，所以範例中另外使用 `webhookEventId` 去重。這兩個機制處理的是不同問題：

- `webhookEventId`：避免同一個 event 被處理兩次。
- `timestamp`：避免不同 edit events 以錯誤順序更新資料。

兩者都需要處理。

## 第二個陷阱：編輯後的內容可能已經不是有效訂單

使用者不一定只修改杯數。他也可能把原本的訂單改成：

```text
我突然不想喝了
```

這時候新的訊息已經無法通過訂單格式驗證。如果後端繼續保留舊訂單，仍然會造成狀態不一致。

這個 Demo 的做法是：清除舊訂單內容，將這筆記錄標成無效並暫時移出總計，再請使用者繼續編輯修正。

```go
parsed, err := parseOrder(text)
if err != nil {
    current.Item = ""
    current.Sugar = ""
    current.Ice = ""
    current.Quantity = 0
    current.UnitPrice = 0
    current.Valid = false
    current.UpdatedAt = timestamp
    buy.Orders[messageID] = current
    return summarize(buy), err
}
```

實際產品也可以採用不同策略，例如把它放進「等待人工確認」的狀態；重點是不要默默沿用已經不存在於聊天室中的舊內容。

## Unsend event：技術之外，還有使用者意圖

Unsend event 的內容比 Edit event 更簡單：

```json
{
  "type": "unsend",
  "source": {
    "type": "group",
    "groupId": "Ca56f94637c...",
    "userId": "U4af4980629..."
  },
  "unsend": {
    "messageId": "610830548529053697"
  }
}
```

它只告訴我們哪一個 message ID 被收回，不會重新附上訊息內容，也沒有 reply token。

因為我們已經用 message ID 當訂單 ID，所以刪除很直接：

```go
delete(buy.Orders, messageID)
```

但這裡真正重要的不是 `delete()`，而是如何尊重使用者「我希望收回這段內容」的意圖。

官方文件特別提醒，服務提供者收到 Unsend event 後，應謹慎處理，讓目標訊息未來無法再被看見或使用。因此這個 Demo 不會在 Bot 回覆中引用被收回的品項、甜度或其他原文，而只會說：

```text
💨 一筆訂單已安全撤回，保存的內容也已刪除。
```

由於 Unsend event 沒有 reply token，如果想主動通知群組，只能使用 push message。這表示它會計入訊息用量，設計正式服務時也要把配額與成本考慮進去。

完整說明請參考 [Messaging API：Unsend event](https://developers.line.biz/en/reference/messaging-api/#unsend-event)。

## 用 Flex Message 讓狀態變化看得見

純文字雖然能顯示結果，但如果要讓 Demo 一眼就懂，我更推薦用 Flex Message 顯示目前訂單。

這次的卡片包含：

- 店家與截單時間
- 團購進行中或已結單
- 每位成員的品項、甜度、冰量、數量與小計
- 總杯數與總金額
- 「重新整理訂單」按鈕

每次新增、編輯或收回訂單後，都重新從目前狀態產生 Flex Message。這樣 edit 前後的杯數與金額會立即變化，unsend 之後該筆明細也會消失。

為了避免 Flex Message 過大，範例最多顯示八筆訂單，其餘筆數用摘要呈現。正式產品可以依需求改用 carousel、LIFF 頁面，或加入分頁查詢。

## SDK 與執行環境版本

本範例使用：

```text
github.com/line/line-bot-sdk-go/v8 v8.22.0
Go 1.25
```

舊版 SDK 雖然已經有 `UnsendEvent`，但不一定包含新的 `MessageEditedEvent` 型別。升級 SDK 後，也要確認它所要求的 Go 版本；這次從舊範例升級時，Go toolchain 與 CI workflow 都需要一起調整。

這是一個常見但容易漏掉的升級問題：本機可以編譯，不代表 Cloud Build 或 GitHub Actions 還在使用相同版本。

## 部署到 Cloud Run 時的狀態問題

這個 Demo 部署在 Google Cloud Run，channel secret 與 channel access token 透過 Secret Manager 注入，沒有寫進 repository。

為了讓範例保持簡單，目前訂單存在程式記憶體。這會帶來兩個限制：

1. instance 重啟或縮容到零，訂單就會消失。
2. 同時存在多個 instances 時，每個 instance 會擁有不同的訂單狀態。

因此 Demo 環境先把最大 instance 設為 1，避免同一場團購的 webhook 被分流。但這只適合展示，不是正式環境的完整解法。

正式服務應該使用 Redis、Firestore 或其他共享儲存，並進一步處理：

- 原子更新與併發控制
- webhook idempotency
- 資料保存期限
- unsend 後的跨系統刪除
- Push Message 失敗重試
- 稽核紀錄與隱私要求之間的界線

尤其是 unsend，如果訊息內容已經被送往搜尋索引、分析平台或其他下游服務，只刪除主資料庫並不完整。資料流設計一開始就應該知道內容去了哪裡，才有能力真正完成刪除。

## 如何執行這個 Demo

首先，請在 LINE Developers Console 啟用 webhook，並允許 Bot 加入群組聊天室。把 webhook URL 指向部署服務的 `/callback`。

接著在群組中依序輸入：

```text
開團 五十嵐 15:20 截單
珍珠奶茶 / 半糖 / 少冰 / 1
目前訂單
```

然後直接編輯第二則訊息：

```text
珍珠奶茶 / 微糖 / 去冰 / 2
```

你會看到 Bot 回覆「訂單變形成功」，Flex Message 的杯數與總金額也同步更新。

最後再收回這則訂單訊息，Bot 會刪除內容並顯示新的訂單總結。

範例中的菜單與價格是固定的 Demo 資料，方便讓金額變化清楚可見。完整啟動方式、環境變數及測試指令請參考 repository 的 README：

<https://github.com/kkdai/linebot-edit-unsend>

## 未來還能怎麼應用？

團購只是其中一個容易理解的故事。相同的事件模型也可以延伸到：

- 行程 Bot：編輯集合時間時更新行程，收回訊息時取消活動。
- 任務 Bot：編輯負責人或期限，收回訊息時移除任務。
- 預約 Bot：修改時段或人數，收回訊息時取消預約。
- 公告 Bot：更新公告內容，收回後同步撤下其他通路的副本。
- 互動故事 Bot：編輯選擇時改寫劇情，收回訊息時退回上一個節點。

只要 Bot 曾經把「一則訊息」轉換成某種系統狀態，就值得重新檢查：當訊息被編輯或收回時，那個狀態是否也應該一起改變？

## 結語

訊息編輯看起來是一個聊天介面的改善，但從 Bot 開發者的角度來看，它其實改變了事件的生命週期。

一則訊息不再只有「被送出」這個瞬間。它可能被更新，也可能被撤回。後端服務需要理解這些事件、維持正確順序，並尊重使用者的最新意圖。

這次我用一個下午茶團購故事，把 Edit event、Unsend event、Flex Message 與 Cloud Run 串在一起。希望這個小範例能讓大家更快掌握新功能，也開始思考自己的 LINE Bot 在訊息被改變之後，應該如何回應。

歡迎參考程式碼，也期待看到大家做出更多有趣的應用：

<https://github.com/kkdai/linebot-edit-unsend>

## 參考資料

- [LINE Premium「編輯訊息」功能於 LINE Labs 開放免費試用](https://www.linecorp.com/tw/pr/news/2026/0820/)
- [LINE Messaging API reference：Edit event](https://developers.line.biz/en/reference/messaging-api/#edit-event)
- [LINE Messaging API reference：Unsend event](https://developers.line.biz/en/reference/messaging-api/#unsend-event)
- [LINE Messaging API 新功能介紹：Mark as Read API 讓你的聊天機器人標記訊息已讀](https://techblog.lycorp.co.jp/zh-hant/linebot-mark-as-read)
