---
layout: post
title: "[分享][Go] 建立一個 Server 幫你把 Twitter 透過 IFTTT 轉發到 Plurk "
description: ""
category: 
- go
tags: ["go", "twitter", "plurk"]

---

![](http://ext.pimg.tw/derek2009/4a0e884470b0d.jpg)


[Plurk](http://www.plurk.com/) 一直是台灣人很愛的推文服務．不過因為只有台灣在使用，所以許多大的服務（比如說 [IFTTT](https://ifttt.com) ) 就不太理他． 之前本來還可以很簡單的透過 Email 來發送噗，結果也不知道為何關閉 （聽說要收費？） ． 於是想了很多方式來弄，都還是有點麻煩． 最後還是自己寫一個 Server 來給 [IFTTT](https://ifttt.com) 使用



如何讓你的 Twitter 轉發到 Plurk ?
=============

### 首先申請一個 Plurk APP

1. 先到這個地方，申請你的 [App](http://www.plurk.com/PlurkApp/)．
	- **類別**:  手機或應用程式
	- 其他相關資料填寫可以參考[這篇文章](http://zh.blog.plurk.com/archives/1121)
2. 點下測試工具
3. 取得以下四個數值
	- App key
	- App secret
	- Token
	- Token secret

在Plurk  App就算是完成，接下來要到 [IFTTT](https://ifttt.com)設定

### 再來架設你自己的Plurk Maker Server

到 [https://github.com/kkdai/plurk-makerserver](https://github.com/kkdai/plurk-makerserver) 按下下面的按鈕

![Deploy](https://www.herokucdn.com/deploy/button.svg)

記住你的 Server URL 等等要使用

#### 在 IFTTT Maker 上的設定

1. 接下來到 [IFTTT Maker](https://ifttt.com/maker) 申請一個帳號．

2. 建立一個 IFTTT Receipt ， 前端用 Twitter 接起來，後面接到剛剛建立的 Maker ．

3. Maker 設定頁面上，資料依照以下的格式來填:

- URL :  你剛剛的 Server URL
- Method: POST
- Content Type: application/json
- Body: 依照以以下的修改，貼上去


```
// 記得將以下資料換成剛剛在 Plurk 拿到的資料
// [[App key]]      -> App key
// [[App secret]]   -> App secret
// [[Token]]        -> Token
// [[Token secret]] -> Token secret
// [[Text]]         -> 改成 twitter 內容 

{"ConsumerToken":"[[App key]]", "ConsumerSecret":"[[App secret]]", "AccessToken": "[[Token]]","AccessSecret":"[[Token secret]]", "Msg": "[[Text]]"}
``` 

這樣就可以了.... 有問題來討論...