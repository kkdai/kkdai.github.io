---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-09-12 13:10:15+00:00
slug: '%e8%ae%80%e6%9b%b8%e5%bf%83%e5%be%97%e4%b9%8b%e4%b8%80-software-estimation-demystifying-the-black-art-best-practices-microsoft%e3%80%80-%e5%ad%b8%e7%bf%92%e5%b0%88%e6%a1%88%e7%ae%a1%e7%90%86'
title: '讀書心得(之一)–Software Estimation: Demystifying the Black Art (Best Practices (Microsoft))　
  學習專案管理的好書~~~~'
wordpress_id: 945
categories:
- 書海裡的漫遊
---

![book.jpg](http://farm4.static.flickr.com/3276/2849223347_614f88764f.jpg)

 

<blockquote>  
> 
> by [Steve McConnell](http://www.evanlin.com/exec/obidos/search-handle-url?%5Fencoding=UTF8&search-type=ss&index=books&field-author=Steve%20McConnell) (Author) 
> 
>    
> 
> Covers software estimation techniques with information on how to successfully estimate scheduling, cost, and project activities. 
> 
>    
> 
> 圖片提供Amazon: [http://www.amazon.com/Software-Estimation-Demystifying-Practices-Microsoft/dp/0735605351](http://www.amazon.com/Software-Estimation-Demystifying-Practices-Microsoft/dp/0735605351)
> 
> </blockquote>

 

 

其實前兩天就把這本書買回來了，由於工作關係，其實到了現在也才看完三分之一而已。不過還是覺得可以先把一些心得做一個簡單的整理。首先先來提提這本書的作者吧。[Steve McConnell](http://www.stevemcconnell.com/) 已經被公認是軟體研發裡面相當具有經驗的人，而他所寫的[Code Complete](http://cc2e.com/)更是聖經中的聖經。不過我個人比較喜歡的書則是[Rapid Development: Taming Wild Software Schedules (Paperback](http://www.amazon.com/Rapid-Development-Taming-Software-Schedules/dp/1556159005/ref=sr_1_2?ie=UTF8&s=books&qid=1221186975&sr=1-2))，因為這本書裡面利用故事講解了許多關於軟體開發上會遇到的現象與問題。

 

回頭來談談買這本書的原因吧，主要是因為最近組織調整的關係，原本我們是負責主要軟體研發與設計的部分(當然有所謂的除錯)。工作調整後，現在則專心在做專案管理與主要產品的控管部分。套句主管常說的話，除錯的能力人人都有，但是專案管理的能力可以讓你一輩子帶著走。不過仔細下去開始做，每個同事都是做得唉唉叫。比起以前硬著頭皮下去改程式碼、修臭蟲，原來專案管理與軟體研發的時間管理是更佳的困難。以前只要開程式開發工具毛起來去解問題，解不出來就加班到死~其他人也跟著你拼到死把產品交出去。現在則要好好的安排各種研發的時間與測試需要的時間與風險管理。如果需要別人加班，還需要有相當大的理由去說服人家~ 你當初在專案管理的時候怎麼沒有把加班排進去你的計畫中。除此之外，每天的報告與分析計畫更是少不了。不過~ 可能是以前做整理與分析資料的工作習慣了，我反而相當適應這樣的工作。


<!-- more -->
  

**心得:**

 

這本書~ 就如同他的標題一樣，主要就是講解如何去對你的軟體開發去做"評估"。怎麼樣才是一個好的評估呢? 在他的第一章先解釋"評估"與"計畫"的關係。(Relationship between estimation and plan)。裡面開宗明義開始解釋一下這本所提到的評估是甚麼，還有不要把評估當成是計畫。因為評估的時候往往是沒有了解全部資訊的，而計畫才是。(當然我們往往犯下錯誤~ 在沒有全部消息的時候就開始做計劃、或是被要求做計劃)。

 

個人相當的喜歡他的第二章想要傳達的重點，你是一個多好的評估者? (How good an estimator are you?) 裡面就有一個小試驗(算事小測驗)如下:

 

Table 2-1: How Good an Estimator Are You?

 

**[Low Estimate - High Estimate] ****Description**

 

[ _______________ - _______________ ] Surface temperature of the Sun

 

[ _______________ - _______________ ] Latitude of Shanghai

 

[ _______________ - _______________ ] Area of the Asian continent

 

[ _______________ - _______________ ] The year of Alexander the Great's birth

 

[ _______________ - _______________ ] Total value of U.S. currency in circulation in 2004

 

[ _______________ - _______________ ] Total volume of the Great Lakes

 

[ _______________ - _______________ ] Worldwide box office receipts for the movie _Titanic_

 

[ _______________ - _______________ ] Total length of the coastline of the Pacific Ocean

 

[ _______________ - _______________ ] Number of book titles published in the U.S. since 1776

 

[ _______________ - _______________ ] Heaviest blue whale ever recorded

 

Source: Inspired by a similar quiz in _Programming Pearls_, Second Edition ([Bentley 2000](BBL0182.html#850)).

 

This quiz is from _Software Estimation_ by Steve McConnell (Microsoft Press, 2006) and is (C) 2006 Steve McConnell. All Rights Reserved. Permission to copy this quiz is granted provided that this copyright notice is included.

 

 

How did you do? (Don't feel bad. Most people do poorly on this quiz!) Please write your score here: _____________

 

(解答請買書吧~~~~ 這本書真的不錯）

 

 

你會怎麼做這個評估，在你不知道以上解答的時候? 而你又被要求須要有90%以上的準確度的時候呢?

 

 

<blockquote>  
> 
> "那~~~ 就設定一個超級大的範圍吧?"
> 
>    
> 
> </blockquote>

 

你的心理或許有這樣黑暗之音會出現~~~但是另外一邊一定也會出現

 

 

 

 

<blockquote>  
> 
> "這麼隨便亂寫~ 好像給人感覺我不專業~~~~"
> 
>    
> 
> "你根本就是亂猜嘛!"
> 
>    
> 
> "怎麼可能這樣寫!!!!"
> 
> </blockquote>

 

 

 

 

這些問題~~~回過頭來做專案評估的時候，我相信各位跟我一樣~~經常會遇到你的專案評估的不確定度 跟你不確定北半球海岸線長度是一樣的。 那你要如何去評估呢？？？？ 在這裡作者沒有一定告訴你怎麼去評估～～他先詢問了所有做過題目的人，發現大部分的人只有對3~4題而已，而少數幾個十題全對的人。 發現她們的預測值都相當的大～～～ 這樣又代表著什麼?? 這裡作者沒有太仔細寫明，但是我個人覺得~ 對於不確定或是不了解的專案~ 過分悲觀的評估比起過分樂觀反而比較容易抓到正確的值。 相較之下~ 在這種情況下~ 如果你的值的範圍都相當的小~ 而你對於你的答案的範圍沒有任何的信心~ 你不過只是亂猜罷了!!

 

 

接下來的第三章~ 就開始來談~~ 精準評估的價值 (The Value of Accurate Estimation)。 有專案管理經驗的人都知道 過分評估(Overestimation)與不足評估(Underestimation)的痛苦。 往往銷售端的人由於客戶壓力就會有不足評估產生出來的行程。(通常是~~好好！！ 明天就給你修好的版本）（好好好！！ 下周一這個功能就可以做完！） 這些不足的評估造就了我們執行者(身為PL或是PM的人)執行上因為又過度不足的評估造成相當趕的狀況。雖然往往最後都是會延遲~ 但是銷售端的人礙於客戶壓力~ 也不斷的上映一樣的戲碼。 

 

不足評估造成的影響可能有哪些?專案延遲~ 銷售業務可能得去道歉、可能得重新評估一個新的時間、甚者~ 可能給了一個完全無法正常使用的軟體給客戶~造成客戶困擾而生意掉單。過分評估會造成的影響相對的小: 工程師因為一個相當久的時程安排~ 而開始偷懶~ 到最後幾天才開始隨便做。 但是相較於不足評估，過分評估的的影響很明顯少了很多。

 

 

第四章談到的是把需求搞清楚是評估最重要的東西，由於我還在看~~ 先寫到這裡!!
