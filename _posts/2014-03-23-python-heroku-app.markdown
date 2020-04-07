---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-23 17:32:45+00:00
slug: python-%e5%a6%82%e4%bd%95%e5%9c%a8heroku-%e4%b8%8a%e9%9d%a2%e5%bb%ba%e7%ab%8b%e7%ac%ac%e4%b8%80%e5%80%8bpython-app
title: '[Python] 如何在Heroku 上面建立第一個Python app'
wordpress_id: 1358
categories:
- Python
- 學習文件
---

之前一直有聽說[Heroku](https://dashboard.heroku.com/apps)的服務相當好用，其實帳號也申請好了．  
但是一直沒有時間把他設定起來，而是一直卡在local端學習Ruby on Rails  
不過最近開始要認真把它弄起來，也要把Python開始好好的補習一下  
這裡稍微記錄一下關於如何在Mac上架設Heroku Python App的筆記






  * 主要流程參考[Heroku官方教學說明](https://devcenter.heroku.com/articles/getting-started-with-python#)，這裡只挑一些會有問題的地方加上註解:


  * 關於Python 環境架設



    * 由於Mac OS本身就有Python，這倒不是太困難的事情．主要是要安裝[VisualEnv](https://pypi.python.org/pypi/virtualenv) (這裡有更多[說明](http://www.openfoundry.org/tw/tech-column/8516-pythons-virtual-environment-and-multi-version-programming-tools-virtualenv-and-pythonbrew))



      * easy_install virtualenv




  *  Heroku login與SSH key



    * 第一次打Heroku login 會自動把在你設定的.ssh/rsa_pub 上傳到server去當做你的key



  * 關於Heroku Toolkit : Procfile 與 foreman



    * Procfile是你需要寫一個設定檔 “Procfile"去執行相關的app


    * foreman 可以讓你local 去執行你要建立在Heroku的app



  * 由於架構上是利用 git 把你local 的檔案上傳到遠端的 [Heroku](https://dashboard.heroku.com/apps) Git server 然後去執行它．所以必須要了解[Git基本指令](http://gitref.org/)，每次改完code可以先用foreman先在本地端預覽，然後再push到[Heroku](https://dashboard.heroku.com/apps)去




好了，這樣也架設好第一個[Heroku](https://dashboard.heroku.com/apps) 第一個App接下來要學習更複雜的Python與更多的應用 




**參考：**






  * Windows Heroku Python 架設 [http://danjog.blogspot.tw/2013/08/heroku-windows.html](http://danjog.blogspot.tw/2013/08/heroku-windows.html)


  * Python DJango Heroku [http://www.openfoundry.org/tw/tech-column/8564-python-django-on-heroku](http://www.openfoundry.org/tw/tech-column/8564-python-django-on-heroku)


  * Heroku with Python [https://devcenter.heroku.com/articles/getting-started-with-python](https://devcenter.heroku.com/articles/getting-started-with-python)


  * Git command [http://blog.longwin.com.tw/2009/05/git-learn-initial-command-2009/](http://blog.longwin.com.tw/2009/05/git-learn-initial-command-2009/)


  * Python VirtualEnv [http://www.openfoundry.org/tw/tech-column/8516-pythons-virtual-environment-and-multi-version-programming-tools-virtualenv-and-pythonbrew](http://www.openfoundry.org/tw/tech-column/8516-pythons-virtual-environment-and-multi-version-programming-tools-virtualenv-and-pythonbrew)


