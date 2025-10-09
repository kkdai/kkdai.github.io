---
layout: post
title: "[數位憑證皮夾] 入門版"
description: ""
category: 
- 學習文件
tags: ["TIL", "數位憑證皮夾"]
---

<img src="../images/image-20251009102618401.png" alt="image-20251009102618401" style="zoom: 50%;" />

(圖片來源： [數位憑證皮夾官方網站](https://www.wallet.gov.tw/zh-tw))

## 前提：

數位憑證皮夾是近幾年數發部推動的一個主要政策，希望是透過數位憑證皮夾來取代大大小小的證件、會員卡跟相關的實體晶片卡片。數位憑證皮夾可以是一單一的一個 App ，甚至可以是讓所有的 App 都來當數位憑證皮夾。

這一篇文章稍微解釋數位憑證皮夾的使用方法，還有如何應用數位憑證皮夾，透過一個場景來建立一個數位憑證的發行方與數個認證方。



## 先來玩一下數位憑證皮夾

首先你需要下載數位憑證皮夾的官方 App (目前 iOS 是 TestFlight 版本)

<img src="../images/image-20251009103231602.png" alt="image-20251009103231602" style="zoom:25%;" />

下載之後，你會發現好像裡面空空的。那是因為你還沒有建立你自己的卡片。 



## 測試記者會的相關流程

打開 App 會看到有以下

<img src="../images/image-20251009113551533.png" alt="image-20251009113551533" style="zoom:25%;" />

建議可以先加入 

- OTP 電子卡（需要輸入電話，收簡訊）
- 駕照電子卡（可以輸入測試資料沒問題）

加入了相關憑證後，就可以透過出示憑證來測試。 建議可以走「超商取貨」的範例來測試一下。

<img src="../images/image-20251009114128661.png" alt="image-20251009114128661" style="zoom: 25%;" />



可以看到，超商領貨需要兩種資料，是可以從兩種數位憑證上面取得。

- 駕照卡 -> 姓名
- OTP卡 -> 電話

然後都不需要其他的欄位，這就是選擇性揭露的原則。



## 數位豆泥卡範例 Web App

網址： [https://mashbeanvc.tonyq.org/](https://mashbeanvc.tonyq.org/)

![Google Chrome 2025-10-09 17.39.55](../images/Google Chrome 2025-10-09 17.39.55.png)

這個 Web App 作為展示有以下兩個主要功能：

- 申請一張豆泥卡（只需要暱稱，生日是選填）
- 幫豆泥點蠟（也就是驗證的意思）



這個範例也充分了應用以下主要 API :

- 發卡方： （參考： [https://issuer-sandbox.wallet.gov.tw/swaggerui/#/](https://issuer-sandbox.wallet.gov.tw/swaggerui/#/)）
  - `/api/qrcode/data` 產生 QR code
- 驗證方： （參考: [https://verifier-sandbox.wallet.gov.tw/swaggerui/#/)](https://verifier-sandbox.wallet.gov.tw/swaggerui/#/))
  - `api/oidvp/qrcode`: 產生驗證的 QR Code...
  - `api/oidvp/result`: 檢查驗證結果是否成功...



## 打造一個簡單數位憑證驗證場景吧

<img src="http://localhost:3000/images/avatar_candle.png" alt="Avatar" style="zoom:50%;" />

接下來要做一個給 HR 的數位員工卡系統：



