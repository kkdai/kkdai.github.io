---
layout: post
title: "[Cloud Platform] 身為 AI (LLM) Engineer 關於雲平台的挑選 Heroku v.s. Render"
description: ""
category: 
- TIL
tags: ["Heroku", "Render", "FaaS"]

---

<img src="../images/2022/image-20240104115649015.png" alt="image-20240104115649015" style="zoom:33%;" />

# 前情提要：

有在持續關注我的部落格的朋友，都知道我喜歡透過 Heroku 來快速部署我自己的雲服務 (或是 LINE Bot)。 當初 Heroku 就打著好上手，並且有免費方案的流程來讓許多開發者都有使用到。但是在 2022 十一月之後，就開始收費的 Heroku 也開啟許多人跑走的路途。這邊可以查看以下相關資料：

- [關於從 Heroku 跳到 Render 這件事情](https://israynotarray.com/other/20221213/3036227586/)

- [五個替代 Heroku  的平台免費測試執行 .Net Core 的文字辨識 OCR 網頁程式](https://blog.user.today/fly-io-asp-net-core-docker-ocr/)

- [從 Heroku 遷移至 Fly.io](https://medium.com/codememo/%E7%AD%86%E8%A8%98-%E5%BE%9E-heroku-%E9%81%B7%E7%A7%BB%E8%87%B3-fly-io-5f9f5cdb837b)

由於我手上的 Hobby Project 大概有 **50~80 個**，所以也就選擇先留在 Heroku 看看？ (要搬移真的太累) 每個月五美金的費用，也算是中規中矩，畢竟也是某些程度吃到飽（1000 dyno hours) ，也足夠我用。

但是又聽到朋友們的提醒與鞭策，於是來認真看一下跟測試結果。



# tl;dr 先講結論

- Heroku ($5) 雖然沒有免費，但是共用的 Eco Dyno 其實後夠力。 Render ($7) 每一個要單獨收，有點貴。
- 我會開始放一些到 Render $0 的方案，原因後提。
- 兩個都部署跟發布，但是我還是會留在 Heroku ($5)  (因為它是共用 1000 hours)



## 價位比較 (based: 2024/01/02)

## Render 的價格跟性能: (free/Starter)

根據 [Render vs Heroku by Render](https://docs.render.com/render-vs-heroku-comparison) 有一些比較表：

![image-20240104095253552](../images/2022/image-20240104095253552.png)

認真看了一下 [Render 的費用](https://render.com/pricing) 

![image-20240104095320382](../images/2022/image-20240104095320382.png)

- 看起來 Free-tier 就有 512MB RAM，很不錯（但是要注意 CPU 0.15)
- 但是要注意 Free Bandwidth 是 100GB ，這個感覺會踩到。

![image-20240104095420088](../images/2022/image-20240104095420088.png)

- 更別說有免費 90 天的 PostgresSQL 資料庫 

![image-20240104095506022](../images/2022/image-20240104095506022.png)

- Redis 有 25MB 也夠用，但是會砍掉。



## Heroku 的費用與效能部分

- 沒有 Free-tier 
- 最低收費 Eco $5

根據 [Heroku Dyno Types](https://devcenter.heroku.com/articles/dyno-types):

![image-20240104095813909](../images/2022/image-20240104095813909.png)

- 效能其實不比 $7 差太多（當然遠遠多於 Render Free-Tier)

![image-20240104095917259](../images/2022/image-20240104095917259.png)

## 快速測試結果

拿了專案 https://github.com/kkdai/linebot-gemini-prohttps://github.com/kkdai/linebot-gemini-pro 來測試，發現：

- 回覆速度上 Heroku ($5) 跟 Render ($0) 不會差太多。 (Golang App)
- 但是上傳照片後，要處理 Render ($0) 就會炸掉。 由於記憶體兩者都是 512MB ，考量可能問題出在 CPU 上面。

### 交叉測試：

加上我另外一個小專案： [https://github.com/kkdai/pdf_online_editor?tab=readme-ov-file](https://github.com/kkdai/pdf_online_editor?tab=readme-ov-file)

<img src="../images/2022/demo.png" alt="img" style="zoom:25%;" />

測試結果 Render ($0) 一樣會炸掉。

### 測試檔案 44.8MB PDF

##### Heroku ($5)

<img src="../images/2022/image-20240104121612188.png" alt="image-20240104121612188" style="zoom:25%;" />

##### Render ($0)

等太久失敗....

<img src="../images/2022/image-20240104121857862.png" alt="image-20240104121857862" style="zoom:25%;" />



## 付費後的測試 ($$$$)

![image-20240104122958983](../images/2022/image-20240104122958983.png)

- 一樣沒反應....... （！！！）
- 好吧！ 悲劇了～～～ 做 RAG 的時候，需要大量的 CPU 這件事情。 Render ($7) 也不能滿足我的需求。

#### Render 是每一個專案收費， Heroku 是共用 1000 hours

- 開了兩個專案，每個預計 $7 。總共預期被收 $12 。這就有點讓我吃驚。

![image-20240104123423199](../images/2022/image-20240104123423199.png)![image-20240104123424548](../images/2022/image-20240104123424548.png)



# 結論

雖然 Heroku ($5)  比起 Render ($7) 還要便宜，但是考量以下的部分，可能會開始同步開啟：

- Render 有免費方案，更適合作為活動推廣之用。 
- Render ($7)  有 DB 可以使用，但是 Heroku ($5)   的資料庫要額外付錢。
- Render ($7)  提供的管理頁面跟相關功能比較多。

但是... 因為 Heroku($5)（CPU) >>>>  Render ($7) ，加上 Render ($7)  是每個專案都要收費。如果開的一多，可能會完全吃不消。

可能要多多考慮...  



# 請推薦給我好用的雲服務



如果有其他好推薦的，請各位留言給我。我的需求如下：

- 由於 Hobby Project 眾多 (30 ~ 50) 希望可以共用費用。 (e.g. 1000 hours 共用)
- 希望有收費上限，不要不小心爆表。
- CPU 希望可以高一點，畢竟 RAG 跟 LLM 需要 CPU 跟 RAM 都不少。

 

# 參考資料：

- [關於從 Heroku 跳到 Render 這件事情](https://israynotarray.com/other/20221213/3036227586/)

- [五個替代 Heroku  的平台免費測試執行 .Net Core 的文字辨識 OCR 網頁程式](https://blog.user.today/fly-io-asp-net-core-docker-ocr/)

- [從 Heroku 遷移至 Fly.io](https://medium.com/codememo/%E7%AD%86%E8%A8%98-%E5%BE%9E-heroku-%E9%81%B7%E7%A7%BB%E8%87%B3-fly-io-5f9f5cdb837b)

- [Render vs Heroku by Render](https://docs.render.com/render-vs-heroku-comparison)
