---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-28 16:31:38+00:00
slug: python-%e5%a6%82%e4%bd%95%e5%9c%a8heroku%e4%b8%8a%e6%9e%b6%e8%a8%addjango-%e5%bf%ab%e9%80%9f%e7%ad%86%e8%a8%98%e8%b7%9f%e9%8c%af%e8%aa%a4%e8%a7%a3%e6%b1%ba%e6%96%b9%e6%b3%95
title: '[Python] 如何在Heroku上架設Django- 快速筆記跟錯誤解決方法'
wordpress_id: 1367
categories:
- Python
- 學習文件
---

自從上次開始重新學習python之後，現在繼續開始把python學習之旅延伸到[Django](https://www.djangoproject.com/)  
學習[Django](https://www.djangoproject.com/) 主要是為了要架設所謂的[RESTful](http://zh.wikipedia.org/zh-tw/REST)的server而鋪路  
主要也是為了之後可以把server架設起來後，不論是透過行動裝置或是其他的伺服器可以抓取到放在[Heroku](https://www.heroku.com)上面的資料  
這邊由於自己也還在摸索，所以對於上列所有的專有名詞請自行點進去觀看




在前一篇文章中，裡面有提到兩個奇怪的指令 [virtualenv](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0CDcQFjAB&url=https%3A%2F%2Fpypi.python.org%2Fpypi%2Fvirtualenv&ei=ynM1U73dJMmflQXN8YGwBw&usg=AFQjCNHM6Xbe2D_WryZ8lIQoS8u_-0qIUQ&sig2=VUgKZ_tnjFZKZZ8D7i5frA) 與 source  與Procfile  
尋找了一下相關的教學文件，這裡可以摘錄一下:




<blockquote>

> 
> Virtualenv 和 Pythonbrew 都是可以創造虛擬（獨立）Python 環境的工具，只是虛擬（獨立）標的不同。  
Virtualenv 可以隔離函數庫需求不同的專案，讓它們不會互相影響。在建立並啟動虛擬環境後，透過 `pip` 安裝的套件會被放在虛擬環境中，專案就可以擁有一個獨立的環境。簡而言之，Virtualenv 可以幫你做到：
> 
> 

> 
> 

>   * 在沒有權限的情況下安裝新套件
> 

>   * 不同專案可以使用不同版本的相同套件
> 

>   * 套件版本升級時不會影響其他專案  
取自: [http://www.openfoundry.org/tw/tech-column/8516-pythons-virtual-environment-and-multi-version-programming-tools-virtualenv-and-pythonbrew](http://www.openfoundry.org/tw/tech-column/8516-pythons-virtual-environment-and-multi-version-programming-tools-virtualenv-and-pythonbrew)
> 

</blockquote>




另外一篇也有提到: 




<blockquote>

> 
> Virtualenv 可以建立虛擬的 Python 環境，虛擬環境彼此之間互不干擾，也可避免搞亂 Python 主要安裝路徑，可以使用 `virtualenv --distribute venv` 來建立一個虛擬環境路徑，其中 venv 可以是任意的環境名稱，這會建立一個 venv 目錄，進入該目錄後，執行 `source bin/activate` 可以啟動虛擬環境，輸入 `deactivate` 可以關閉虛擬環境。
> 
> 

> 
> [http://www.codedata.com.tw/python/python-tutorial-the-1st-class-3-hello-world-traditional-chinese-edition/](http://www.codedata.com.tw/python/python-tutorial-the-1st-class-3-hello-world-traditional-chinese-edition/)
> 
> 
</blockquote>




所以依照這兩種意思，可以清楚地了解為了不要混淆到各個專案的套件，所以可以利用[virtualenv](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0CDcQFjAB&url=https%3A%2F%2Fpypi.python.org%2Fpypi%2Fvirtualenv&ei=ynM1U73dJMmflQXN8YGwBw&usg=AFQjCNHM6Xbe2D_WryZ8lIQoS8u_-0qIUQ&sig2=VUgKZ_tnjFZKZZ8D7i5frA)來做個簡單的隔離．




**快速筆記**




這裡相當推薦[CoreData這篇文章](http://www.codedata.com.tw/python/python-tutorial-the-4th-class-1-django-getting-started/)，不論是他的[Django](https://www.djangoproject.com/)版本比較不容易有相容性的問題，之後也有簡單的介紹關於[RESTful](http://zh.wikipedia.org/zh-tw/REST)與Python架構的文章．  
我這裡只註解一些比較容易忽略的地方






  * 小心使用另外一篇[OpenFoundry的文章](http://www.openfoundry.org/tw/tech-column/8564-python-django-on-heroku)由於他的設定文件都是針對[Django](https://www.djangoproject.com/) 1.4.2 如果你的環境比較新．在設定資料庫就會發生問題．


  * 建議的循序漸進的方式如下:



    * 先用python 架設起來，並且可以正確的使用 python manage.py runserver正常執行．


    * 在開始設定Heroku 的檔案 Procfile 或是requirements.txt然後用 foreman start來測試是不是可以正常執行．


    * 最後才用heroku create 跟 git 來push 到Heroku 主機上面去．



  * 這理由[這篇文章](http://www.realpython.com/blog/python/migrating-your-django-project-to-heroku/#.UzVT9q2Sy3o)有提供教學(當然[Heroku官方也有](https://devcenter.heroku.com/articles/getting-started-with-django)) 裡面有提供 Procfile 的設定



    * web: python manage.py runserver 0.0.0.0:$PORT —noreload 





 




**git push heroku master 發生錯誤時候的解決方法:**









  * No such app as XXXXX-XXXX



    * 原因:



      * 可能是因為執行heroku create兩次



    * 解法:



      * 首先，查看git remote 設定



        * git remote -v  



      * 確定之後移除



        * git remote rm heroku



      *  然後加上新建立的id



        * git remote add heroku YOUR_APP_ID



      * 參考： iT邦幫忙 [http://ithelp.ithome.com.tw/question/10061577](http://ithelp.ithome.com.tw/question/10061577)




  * Push rejected, no Cedar-supported app detected



    * 原因:



      * 這個東西我找了很久，原因可能是任何一種．這邊提供檢查方法跟解決方法．



    * 解法：



      * 先確認local tool “foreman start” 是可以正常執行，來確認 Procfile 跟 requirements.txt沒有任何問題...



        * 如果發生問題，在去這兩個檔案去尋找問題點．



      * 接下來可能是因為你已經開啟了第二個heroku app，由於heroku只能接受開啟一個app，去Heroku控制台去把另外的都砍掉．然重新試試看．









 
