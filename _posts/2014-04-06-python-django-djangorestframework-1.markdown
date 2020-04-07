---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-04-06 12:15:38+00:00
slug: pythondjango%e9%96%8b%e5%a7%8b%e5%ad%b8%e7%bf%92djangorestframework-1
title: '[Python][Django]開始學習DjangoRestFramework (1)'
wordpress_id: 1369
categories:
- Django
- Python
---

對於如何架設一個可以使用JSON來讀取與存取資料的[Django   
](https://www.djangoproject.com/)如果要快速的可以使用JSON的部分，可以去使用[DjangoRestFramework](http://www.django-rest-framework.org/) 




這裡稍微記錄一下，整理流程: 可以參考[http://www.django-rest-framework.org/tutorial/quickstart](http://www.django-rest-framework.org/tutorial/quickstart)






  * 建立準備環境



    * virtualenv env


    * source env/bin/activate


    * pip installl django


    * pip install djangorestframework



  * 開啓project



    * django-admin.py startproject tutorial


    * cd tutorial


    * python manage.py startapp quickstart



  * 修改Project設定



    * edit tutorial/setting.py (這裡要注意一下, database 的Name跟Engine是否對應)  


    
    <code style="padding:0;font-family:Monaco, Menlo, Consolas, 'Courier New', monospace;color:inherit;border-top-left-radius:3px;border-top-right-radius:3px;border-bottom-right-radius:3px;border-bottom-left-radius:3px;background-color:transparent;border:0;"><span style="color:#48484c;" class="pln">DATABASES </span><span style="color:#93a1a1;" class="pun">=</span><span style="color:#93a1a1;" class="pun">{</span><span style="color:#dd1144;" class="str">'default'</span><span style="color:#93a1a1;" class="pun">:</span><span style="color:#93a1a1;" class="pun">{</span><span style="color:#dd1144;" class="str">'ENGINE'</span><span style="color:#93a1a1;" class="pun">:</span><span style="color:#dd1144;" class="str">'django.db.backends.sqlite3'</span><span style="color:#93a1a1;" class="pun">,</span><span style="color:#dd1144;" class="str">'NAME'</span><span style="color:#93a1a1;" class="pun">:<strong>os.path.join(BASE_DIR, 'db.sqlite3'</strong></span><span style="color:#93a1a1;" class="pun">,</span><span style="color:#dd1144;" class="str">'USER'</span><span style="color:#93a1a1;" class="pun">:</span><span style="color:#dd1144;" class="str">''</span><span style="color:#93a1a1;" class="pun">,</span><span style="color:#dd1144;" class="str">'PASSWORD'</span><span style="color:#93a1a1;" class="pun">:</span><span style="color:#dd1144;" class="str">''</span><span style="color:#93a1a1;" class="pun">,</span><span style="color:#dd1144;" class="str">'HOST'</span><span style="color:#93a1a1;" class="pun">:</span><span style="color:#dd1144;" class="str">''</span><span style="color:#93a1a1;" class="pun">,</span><span style="color:#dd1144;" class="str">'PORT'</span><span style="color:#93a1a1;" class="pun">:</span><span style="color:#dd1144;" class="str">''</span><span style="color:#93a1a1;" class="pun">}</span><span style="color:#93a1a1;" class="pun">}</span></code>






  * 設定資料庫



    * python manage.py syncdb



  * 修改其他的部分就繼續參考原來的說明部分



    * [http://www.django-rest-framework.org/tutorial/quickstart](http://www.django-rest-framework.org/tutorial/quickstart)



  * 啟動測試:



    * Python manage.py runserver


    * 直接打開 http://127.0.0.1:8000/users 就可以看結果





接下來應該會弄更複雜的部分，還有去把RESTful關於 post 的部分弄起來..... 
