---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-04-22 01:42:32+00:00
slug: pythondjango-%e8%87%aa%e5%b7%b1%e9%96%8b%e5%a7%8b%e5%af%ab%e7%ac%a6%e5%90%88restful%e7%9a%84server%e8%88%87client2
title: '[python][django] 自己開始寫符合RESTful的server與client(2)'
wordpress_id: 1382
categories:
- Django
- Python
---

其實比起架設RESTful的django伺服器，撰寫一個client去對它存取似乎是簡單多了．  
要用到的東西也很少，因為Python本身對於JSON的處理就相當的簡單．  
所有的處理都是使用到原先架設好的伺服器裡面的資料 http://sleepy-plateau-3929.herokuapp.com/snippets/.json

<!--more-->


這裡稍微提一下GET需要注意的事情






  * 可以使用urllib2的urlopen來讀取網頁


  * JSON有個直接可以使用的json.load


  * 處理過來如果一開始是object可以使用 result[“object”] 來找，反之可以用 list 去處理 List Data


  * context 的部分，可以處理多個以上的context



    * context = {“context_1” :value1, “context_2” : value2}





接下來直接看code




 




POST需要注意的事情






  * 要存取網頁上面的表單資料(form)其實很簡單



    * request.POST[“value_name”]



  * 表單(form)的樣板(template)記得要有[csrf_token](https://docs.djangoproject.com/en/dev/ref/contrib/csrf/)


  * 先確認你的伺服器支援post 的RESTful Add，如果要上傳JSON你的Content-Type要對


  * [Python requests](http://docs.python-requests.org/en/v0.10.7/user/quickstart/) 是個好用的library，一定要用




接下來看code




 




 




最後完整的[github](https://github.com/kkdai/DjangoREST_Client)在這裏




 




 
