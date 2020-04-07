---
layout: post
title: "[Golang][FB] 教學如何快速透過 Golang 與 Heroku 架設一個臉書機器人"
description: ""
category: 
- 
tags: ["go"]
---

![](http://contents.gunosy.com/4/13/6e708e857cab0a34c43f6929e2fc3bf0_content.jpg)

### 1. Just Deploy the same on Heroku

![Deploy](https://www.herokucdn.com/deploy/button.svg)

Remember your heroku ID and app address. ex: `https://APP_ADDRESS.herokuapp.com/`

### 2. Create a Facebook Page

Make sure you already have Facebook account, if you need use FB Bot.


![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot1.png)

### 3. Go to Facebook Developer to create App

Create App, need select as follow:

- New App type [Web App], create app
- Add new product [Messenger]

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot2.png)


### 4. Configuration Messenger Bot

Get token from Faccebook page.

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot4.png)

- Select a "Page" you own.
- Go to "Meseenger" product.
- It will generate `token`
- copy it and store it.

### 5. Paste Token to Heroku

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot5.png)

Go to heroku dashboard, go to "Setting" -> "Config Variables".

- Add "Config Vars"
- Name -> "TOKEN"
- Value use  `token` facebook app.

### 6. Back Facebook App configuration

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot6.png)

- Go to "Messenger" product
- Go to "Setup Webhooks"
- Fill `https://APP_ADDRESS.herokuapp.com/webhook` in callback URL.
- Fill your `token` in "token".
- Checked the checkbox "message_deliveres", "messages".

If your configuration is correct, it will show "completed".

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot7.png)

Also remember to choose correct page in this setting.


### 7. Request basic permissions

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot8.png)
 
In Messenger application review, press "Request Premission".
 
- Checked the "pages_messaging".



## How to testing it.

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot9.png)

- Go to your spcific "page" in Facebook.
- Press "Send Message"

![](https://github.com/kkdai/FBBotTemplate/raw/master/images/Bot10.png)


## How to modification your Bot Code

- Download code from Heroku `git clone https://git.heroku.com/APP_ADDRESS.git`
- Modify code on `main.go` first. especially in `MessageReceived()`.
- Commit and push it back to heroku `git push heroku master`.

