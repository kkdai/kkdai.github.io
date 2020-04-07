---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-04-16 10:43:21+00:00
slug: pythondjango-%e8%87%aa%e5%b7%b1%e9%96%8b%e5%a7%8b%e5%af%ab%e7%ac%a6%e5%90%88restful%e7%9a%84server%e8%88%87client1
title: '[python][django] 自己開始寫符合RESTful的server與client(1)'
wordpress_id: 1380
categories:
- Django
- Python
- 學習文件
---

**前言：**




前幾次開始了Django設計之後，雖然弄出了RESTful的網站，也使用了[DjangoRESTFramework](http://www.django-rest-framework.org/)．但是，都是依照範例程式來跑出來的．  
所以現在決定除了裡面的範例程式之外，另外在同一個網站裡面弄了一個部分是模擬遠端連線  
[依照著tutorial 1~3](http://www.django-rest-framework.org/tutorial/3-class-based-views) 去修改，所以他的架構如下:






  * tutorial (主要的專案部分)


  * snippet (RESTful web site)



    * 負責RESTful的部分，裡面都是使用到[DjangoRESTFramework](http://www.django-rest-framework.org/)所提供的function.


    * 主要的API 是 snippet



  * webs (模擬的viewer遠端)



    * 主要這裡就是這次開發的部分，主要提供兩個部分:



      * 一個是顯示所有資料的的部分(json get) ，其中全部顯示已經完成但是顯示詳細的部分還沒有．


      * 另外一個是新增資料(使用 json post)(尚未完成)






**遇到的問題集錦:**  
這裡整理一下 ，我這次做出來所遇到的一些問題:






  * **字串的編碼問題**



    * 其實這種問題算是基本的，而且也跟我在中文系統上面執行python有關．


    * json出來的資料想要parse出來會成為一堆list


    * 每個list裡面的格式會是utf8~如果要在中文上面正確顯示成字串需要decode(‘big5’)~據說Heroku 預設是utf8但是我使用big5還是可以跑．這個要持續觀察．



  * **樣板的目錄設定(TemplateDoesNotExist)**



    * 問題狀況:



      * TemplateDoesNotExist: 500.html



    * 主要原因：



      * 之前在設定的時候都只有一個project 叫做tutorial．但是這次另外建立了一個webs所以template的目錄會出現問題．


      * 依照之前的經驗會以為templates資料夾應該就放在目錄下的/templates/app/，結果因為webs是第二個專案．所以template的設定會維持在第一個，也因為我並沒有在setup.py裡面把TEMPLATE_DIRS設定好．所以會發生這樣的錯誤．



    *  解決方法：



      * 在setup.py 加上以下的部分  
PROJECT_PATH = os.path.realpath(os.path.dirname(__file__))  
TEMPLATE_DIRS = (  
PROJECT_PATH + '/templates/'  
)


      * 接下來把原本打算放在webs/templates的檔案移到tutorial/template/







    * 參考:



      * [http://stackoverflow.com/questions/4705962/need-help-with-django-tutorial](http://stackoverflow.com/questions/4705962/need-help-with-django-tutorial)






最後再寫一次如何把你的Django Code上傳到heroku









  * python manager.py runserver



    * 首先要確定你可以正常的執行



  * 產生Procfile檔案，其內容為



    * web: python manage.py runserver 0.0.0.0:$PORT --noreload



  * heroku login


  * foreman start



    * 確認沒有錯誤訊息


    * 利用 localhost:5000 查看如果任何功能有問題



  * pip freeze > requirements.txt


  * vim .gitignore



    * 確認把virtualenv 產生的目錄加入


    * *.pyc



  * git init


  * git commit -m “任何comment”


  * heroku create


  * git push heroku master


  * heroku ps:scale web=1



    * 記得要先把其他的app dyno設成0或是砍掉～因為沒有付錢的時候heroku只允許dyno=1



  * heroku ps


  * heroku open


  * 其實成功之後，之後就可以用其他比較熟悉的git client比如說是[SourceTree](http://www.sourcetreeapp.com/)不需要在console mode的下指令．







查看結果：




Server:  [http://sleepy-plateau-3929.herokuapp.com/snippets/](http://sleepy-plateau-3929.herokuapp.com/snippets/)




Client:   [http://sleepy-plateau-3929.herokuapp.com/webs/  
](http://sleepy-plateau-3929.herokuapp.com/webs/)Github:  [https://github.com/kkdai/DjangoREST_Client/](https://github.com/kkdai/DjangoREST_Client/commits/master)




 




先寫到目前的狀況～之後會把POST的部分完成當成第二個部分～～～   
