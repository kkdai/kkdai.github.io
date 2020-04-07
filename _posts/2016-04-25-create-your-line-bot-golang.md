---
layout: post
title: "[Golang][教學] 在 Heroku 建立你自己的 LINE 機器人 (LINE Bot API)[更新: 2018/08/15]"
description: ""
category: 
- 製作你的 IM BOT
tags: ["go", "bot"]
---

![](../images/2016/BIB.png)

**更新**:  由於 `GitHub.com/line/line-bot-go/line-bot` 底層更新，相關 vendor 也更新了．

## 前提

LINE 推出了機器人 API ，並且透過(幾乎不審核) 的方式來開放機器人的功能．  大家可以來試試看．

## 如何建立自己的 LINE Bot 機器人

### 1. 先去 LINE 官方網站申請機器人帳號 (LINE Bot API)

![](https://scdn.line-apps.com/n/_5/partner-center/img/lp/bottrial-icon.png)

先到 [這個地方](https://business.line.me/zh-hant/services/bot) 去申請．[更新: 11/15 目前可以開放給開發者使用，沒有限制個數]

### 2. Deploy 基本的 Golang LINE Bot Template

記得到 [https://github.com/kkdai/LineBotTemplate](https://github.com/kkdai/LineBotTemplate) 然後點選下方的 Deploy 按鈕，將基本的程式碼 Deploy 到你的 heroku 之中．

[![Deploy](https://www.herokucdn.com/deploy/button.svg)]()

Remember your heroku, ID.

### 3. 回到 LINE Bot Dashboard 設定基本資料

到你的 "basic account information" 來設定，以下一些資料需要填好:

- `Callback URL`: https://{YOUR_HEROKU_SERVER_ID}.herokuapp.com:443/callback

以下的資訊記得抄起來，等等到 Heroku 設定頁面要填寫

- Channel Secret
- Channel Access Token

### 4. 回到 Heroku 設定相關環境變數

- 到控制台 (dashboard)
- 到 "Setting"
- 到 "Config Variables", 新增下列環境參數:
	- "ChannelSecret"
	- "ChannelAccessToken"

好了... 加入你的機器人．開始跟他講話吧．

### 範例程式碼： [https://github.com/kkdai/LineBotTemplate](https://github.com/kkdai/LineBotTemplate)

這份程式碼是最簡單的範例，設定好之後他只會重複你打的文字．更多的功能會放在另外一份．


## 影片教學

可以根據以下影片的教學來看如何在兩分鐘之內部署自己的 LINE Bot

<iframe width="560" height="315" src="https://www.youtube.com/embed/xpP51Kwuy2U" frameborder="0" allowfullscreen></iframe>


想要修改代碼嗎？參考以下的影片教學吧

<iframe width="560" height="315" src="https://www.youtube.com/embed/ckij73sIRik" frameborder="0" allowfullscreen></iframe>

## 還有任何問題?

### 在這裡留下你的問題，或是直接到 [gitter 上面來討論](https://gitter.im/kkdai/LineBotTemplate?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)．

## 參考鏈結:

- [Golang (heroku) で LINE Bot 作ってみる](http://qiita.com/dongri/items/ba150f04a98e96b160e7)
- [LINE BOT をとりあえずタダで Heroku で動かす](http://qiita.com/yuya_takeyama/items/0660a59d13e2cd0b2516)
- [阿美語萌典 BOT](https://github.com/miaoski/amis-linebot)