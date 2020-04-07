---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-23 14:25:24+00:00
slug: windows-phonec%e6%9c%80%e8%bf%91%e5%af%ab%e4%ba%86%e4%b8%80%e5%80%8bwindows-phone%e7%9a%84google-reader-app
title: '[Windows Phone][C#]最近寫了一個Windows Phone的Google Reader APP'
wordpress_id: 1157
categories:
- NET程式設計
- 學習文件
---

本篇文章應該會用中文撰寫到完，接續著我前幾篇的文章，我一共寫了幾篇跟Google Reader溝通有關的文章。也讓我來仔細敘述當初為何會有這些文章的出現吧。

 

大概是兩個月前[XDite](http://blog.xdite.net/)大大在plurk po了一篇 "我要在一個月之內把[Reeder](http://reederapp.com/)幹出來" (大概是這樣吧!雖然我忘記原文了，而且大大好像後來也刪除了)。其實那一篇我看了以後很震撼~ 對喔!! 可以利用完全仿製另外一個商用APP的方式來練功。

 

於是我也開始撰寫APP，由於我自己只有iPhone，但是卻沒有任何MAC的電腦可以給我提供開發的環境，也只好拿公司最容易找到工具。Microsoft的 Visual Studio來撰寫Windows Phone。

 

一路上從四月才開始把專案建起來到現在，從完全不會寫Windows Phone到搞定Data Binding、知道如何使用Lambda與Delegate、也慢慢把Windows Phone原件搞懂，當然好久沒摸的WPF也開始拿回來K了一陣，到最後的LINQ、JSON(這個方便應該之後會寫一篇文章來做個說明)。其實寫好一個Google Reader App能夠學習的APP技巧真的不少，在這裡也稍微列一下:

 

  
  * 為了登入Google API的服務， 一開始要學習的就是如何去處理網路溝通的方式，面對的就是 HttpWebRequest與WebClient。 當然[這篇文章](http://www.dotblogs.com.tw/pou/archive/2010/10/17/18403.aspx)也幫助我很多。在這時候一併就把Lanbda與Delegate 的觀念學起來，畢竟你得要處理threading與UI呈現的問題。 
   
  * 登入Google Reader API之後就沒事情了嗎? 還要好好的去處理溝通過來得資料，這裡你當然可以乖乖的使用XML的方式或是直接跳到[JSON](http://json.codeplex.com/)的方式。 還好之前學習的Linq與JSON就派上用場。不過Google Reader的JSON資料也怪怪的~ 為了去處理這些東西~也讓我對JSON的觀念更佳的清楚了。 
   
  * 抓回來的Label列表與文章列表要如何處理呢?這時候又要跟WPF裡面的[ListBox](http://msdn.microsoft.com/en-us/library/system.windows.controls.listbox(v=vs.95).aspx)開始奮戰，這時候你又得跟Data Binding還有WPF XAML的語法來奮鬥。 
   
  * 別忘了最後~ 由於你需要離線閱讀還以記錄使用者的登入資訊。這時候又要跟[Local database on Windows Phobne](http://msdn.microsoft.com/en-us/library/hh202860(v=vs.92).aspx)來奮鬥。不過這個算簡單的，畢竟LinQ步算是太難入門的東西。 
 

到目前為止利用自己上班剩餘時間，大概忙了三個禮拜的結果，目前已經可以登入與看文章。由於在今天好不容易把Google Reader裡面的設定閱讀(mark it as read)也搞定了。 現在也才可以寫篇文章做點記錄。不過接下來才是[Reeder](http://reederapp.com/)令人激賞的好用地方，我也先把自己的TODO 列一下:

 

  
  * 順暢與讀體驗與文章下載(裡面包含著完全不影響使用者閱讀的離線下載) 
   
  * 將你喜歡的文章轉載到各個社群媒體Facebook，Evernote…. 等 (希望懂了JSON可以讓之後的路更順暢) 
   
  * 使用者介面得更加美化 (這是我的盲點~~~~~) 
 

好啦!! 今天也總算把基本功能都完成了! 繼續努力了~~~ 
