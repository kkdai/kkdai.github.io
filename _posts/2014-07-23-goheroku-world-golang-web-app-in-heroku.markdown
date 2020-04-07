---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-23 08:36:11+00:00
slug: goheroku-%e7%ac%ac%e4%b8%80%e5%80%8bhello-world-golang-web-app-in-heroku
title: '[Go][Heroku] 第一個Hello world Golang  Web App in Heroku'
wordpress_id: 1466
categories:
- Golang
---

依照以下兩篇應該可以順利成功






  * [http://mmcgrana.github.io/2012/09/getting-started-with-go-on-heroku.html](http://mmcgrana.github.io/2012/09/getting-started-with-go-on-heroku.html)


  * [http://yinghau76.github.io/2013/12/15/go-on-heroku/](http://yinghau76.github.io/2013/12/15/go-on-heroku/)




不過我真的太不熟所有有些地方一直搞混，這裡記錄一下我發生的問題：






  * 千萬記得把你的Go的程式碼放在$GOPATH/src 下，根據[這一篇](http://confreaks.com/videos/3434-gophercon2014-best-practices-for-production-environments)建議，可以考慮放在 $GOPATCH/src/github.com/YOUR_GITHUB_ACCOUNT/ 下面


  * 記得 Procfile 裡面要寫的是你的目錄(也就是編譯好執行檔)名稱  ，不確定的可以用 which 去double confirm




其他參考文章：






  * Go MongoDB and MongoHQ [http://blog.joshsoftware.com/2014/02/28/a-simple-go-web-app-on-heroku-with-mongodb-on-mongohq/](http://blog.joshsoftware.com/2014/02/28/a-simple-go-web-app-on-heroku-with-mongodb-on-mongohq/)


  * Build test using wrecker with go [http://blog.wercker.com/2013/07/10/deploying-golang-to-heroku.html](http://blog.wercker.com/2013/07/10/deploying-golang-to-heroku.html)




接下來會努力 RESTful 還有一些資料庫應用....
