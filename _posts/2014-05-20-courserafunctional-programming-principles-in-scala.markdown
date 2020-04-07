---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-05-20 04:44:36+00:00
slug: courserafunctional-programming-principles-in-scala-%e9%97%9c%e6%96%bc%e8%aa%b2%e7%a8%8b%e5%ad%b8%e7%bf%92%e5%bf%83%e5%be%971
title: '[coursera][Functional Programming Principles in Scala] 關於課程學習心得(1)'
wordpress_id: 1407
categories:
- coursera
- scala
---

偶然的狀況下，知道其實coursera 有在教 scala 的課程，於是多花一點自己的時間來學習．  
其實主要的課程時間並不會花很多，就如同他所講的，一個禮拜頂多一兩個小時看video leture  
但是assignment 可就不是開玩笑的了，每一次的assignment 都花了我好幾個晚上熬夜慢慢地看  
目前到了week4 的階段，大概算是剛剛進入FP的世界，有一些心得可以分享:






  * 千萬要注意assignment 的 instruction



    * 或許也只有我吧，每次拿到作業就打開來想寫．結果就發現沒parameter或是沒有variable．其實都是因為沒有好好的看instruction 產生的．



  * 用Functional Programing 來思考 (Think with FP)



    * 由於我很習慣邊寫測試程式邊寫程式 ([TDD is dead?](http://wp.xdite.net/?p=2478) XD) 所以這邊很容易遇到困難點


    * 幾個推薦的方式是回過頭去慢慢思考set 與 list 的本質，不要被  p => Boolean 搞破頭





 




這裡也記錄一些課程摘要，有興趣可以考慮修修看，畢竟是scala的創辦人開的課（法國人講英文沒那麼難懂)






  * week 0



    * 主要是sbt應用跟簡單的scala 程式架構



  * week 1 



    * 主要是recursive的programming有 Pascal’s Triangle，Parentheses Balancing 跟 Counting Change


    * 我覺得主要是換回你對於recursive programming 的熟悉程度



  * week 2



    * 一些關於set 的操作，不論是 union 還是 contains 


    * 這邊就開始考驗著Functional Programming 的概念了． 讓我相當痛苦.... 所以為了能夠更了解整個架構～我寫了很多的unit test case 並且把整個結果在main 裡面都印出來看．才能有點了解



  * week3



    * 感覺這次的比較實用，利用Set 與 List 來完成 TweetReader來尋找目前趨勢 Trending ，算是相當有用的課程，但是也很難．


    * 因為一開始整個程式碼架構相當的大，極度推薦把instruction 好好看完，再開始看要完成的部分．跟著instruction 一步步把需要完成的完成，會比較容易....





 弄了兩個禮拜～總算搞懂 week3 接下來要繼續努力 … week4...
