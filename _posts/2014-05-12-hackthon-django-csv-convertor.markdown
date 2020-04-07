---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-05-12 01:06:09+00:00
slug: '%e6%98%af%e5%80%8b%e6%8c%ba%e5%a4%a7%e7%9a%84%e5%95%8f%e9%a1%8c%ef%bc%88%e4%b9%9fpythonopendata-%e8%a9%a6%e5%81%9a%e4%b8%80%e5%80%8b%e6%8a%8acsv%e8%88%87xml%e8%bd%89%e6%88%90json%e7%9a%84converter'
title: '[OpenData]在駭客松前試做一個把csv與xml轉成json的converter (csv/xml parser) 主要在解決big5/utf8
  編碼問題'
wordpress_id: 1391
categories:
- Django
- Python
- 學習文件
---

最近為了要參加OpenData的[Shareable Cities駭客松](http://www.accupass.com/go/city10hackathon/)，也為了好好練習一下Python．決定從頭到尾都要使用python當做我主要的程式語言．  
比賽中一開始以為資料都來自于[政府的OpenData網站 data.gov.tw](http://data.gov.tw/)，所以去觀察了一下這個網站裡面的資料，發現有以下的一些問題：






  * 資料格式不統一：



    * 可以看到政府單位的Open Data  有 XML，CSV與JSON 三種資料格式



  * 資料內容格式不統一：



    * 先提到所謂的內容的格式，也就是編碼(encoding)不統一．有的檔案用big5 有的用utf8，這在 python(2.7)與 Ruby (2.1) 是很大的問題，也是這次主要解決的問題．



  * 資料內容不統一: 



    * 有些資料第一列是敘述欄位，有的時候卻是直接開始資料．





所以，我在比賽前想先做出一些工具，可以在比賽當場使用:









  * 統一輸出JSON資料，並且統一編碼為utf8


  * 可以轉換CSV與 XM: 到JSON


  * 可以解決 big5/utf8的編碼問題


  * 可以是情況的把前面幾列忽略掉，輸出更漂亮的資料格式







於是在比賽的前一個禮拜，開始做這個工具．主要會用到以下的python libraries:






  * [PyQuery](https://pypi.python.org/pypi/pyquery): 主要是拿來做為xml 的parser，相當好用．


  * [request](http://docs.python-requests.org/en/latest/)  : 主要拿來作為網路資料的下載，一般人常用的url lib 是不錯用，只是有時候遇到長網址會出現問題．


  * csv, json, os 這些必用的  python libraries 就不詳細敘述




這裡記錄一些在做工具的時候發生的事情:






  * Encoding error的問題



    * 原因:



      * 有的檔案用big5 有的卻用utf8，這裡影響兩個地方，一個是csv的資料讀入．一個是json資料dump



    * 解法：



      * 會試著去使用各種預先列出的encoding 方式，然後傳回正確的結果






 






 







    * 參考:



      * [http://stackoverflow.com/questions/15918314/python-detect-string-byte-encoding](http://stackoverflow.com/questions/15918314/python-detect-string-byte-encoding)




  * Error: 在Mac 上面想要安裝 PyQuery 會發生錯誤  "unknown argument: '-mno-fused-madd’"



    * 原因: 



      * 似乎是 python 2.7 的問題(regression?) 除了等官方正式修復之外～請用以下解法



    * 解法：



      * sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future python setup.py install



    * 參考:



      * [http://bbs.csdn.net/topics/390767158](http://bbs.csdn.net/topics/390767158)


      * [http://stackoverflow.com/questions/22313407/clang-error-unknown-argument-mno-fused-madd-python-package-installation-fa](http://stackoverflow.com/questions/22313407/clang-error-unknown-argument-mno-fused-madd-python-package-installation-fa)




  * Error: [Heroku] ! [remote rejected] master -> master (pre-receive hook declined)



    * 原因:



      * 主要是Procfile 內容有問題



    * 解法:



      * 先去查詢錯誤log  “heroku logs” 發現問題在 “Error R10 (Boot timeout) -> Web process failed to bind to $PORT within 60 seconds of launch”


      * 於是去修改 Procfile 從  



        * web: python manage.py runserver


        * 到


        * web: python manage.py runserver "0.0.0.0:$PORT”





  * Error: renamed heroku app from website, now it's not found



    * git remote rm heroku 


    * git remote add heroku git@heroku.com:yourappname.git


    * Refer: [http://stackoverflow.com/questions/7615807/renamed-heroku-app-from-website-now-its-not-found](http://stackoverflow.com/questions/7615807/renamed-heroku-app-from-website-now-its-not-found)





 最後列一下這邊的[github](https://github.com/kkdai/Django-OpenDataParser) 與 [Heroku](http://opendata-django-parser.herokuapp.com/) (還有[xml](http://opendata-django-parser.herokuapp.com/xml)的) 資料




 




最後記錄一下駭客松(Hackthon)初體驗的心得:









  * 最後其實不用交出實作，只要prototype 看起來有work 就好 (點下去會跑貼圖的app?)


  * 各種事先準備工具真的很重要，程式主體應該只是在當場組合就好


  * 如果要用backend 應該要有frontend  的部分一起準備


  * 其實偷做才是王道，當場只要討論slide表現重點比較好． 







**參考資料:**






  * [http://legacy.python.org/dev/peps/pep-0263/](http://legacy.python.org/dev/peps/pep-0263/)


  * XML encoding: [http://twigstechtips.blogspot.tw/2013/06/python-lxml-strings-with-encoding.html](http://twigstechtips.blogspot.tw/2013/06/python-lxml-strings-with-encoding.html)


  * Json Validator [http://jsonformatter.curiousconcept.com/](http://jsonformatter.curiousconcept.com/)


