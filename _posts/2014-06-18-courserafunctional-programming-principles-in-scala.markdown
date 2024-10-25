---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-06-18 03:38:31+00:00
slug: courserafunctional-programming-principles-in-scala-%e9%97%9c%e6%96%bc%e8%aa%b2%e7%a8%8b%e5%ad%b8%e7%bf%92%e5%bf%83%e5%be%972%e5%ae%8c
title: '[coursera][Functional Programming Principles in Scala] 關於課程學習心得(2)(完)'
wordpress_id: 1416
categories:
- scala
- coursera
---

這是接下來寫幾個禮拜的學習心得．

其實我不知道是課程安排的問題，還是我習慣邊寫邊測試．到後面幾個禮拜，因為比較容易用worksheet去撰寫．整個變得相當容易寫而且快速．

* Week4: 撰寫霍夫曼樹的處理函示

  * 主要內容是關於List的操作，但是比較奇怪的是關於List操作的課程卻是在week5的影音課程裡面 O_o

  * 因為直接拿List來處理，搭配著patch matching其實很容易就上手．這裡可以建議各位多使用worksheet來當你學習上的好朋友，可以快速上手並且測試你的想法．

  * 我的例子就是由於有worksheet可以使用，馬上就著手去開發相關的部分，並且一方面也可以馬上測試來看結果．（順便多寫一些unit test）這樣寫起來應該很快又可以寫完（不過我也是花了接近10+個小時）

  * 最後結果雖然答案都寫完了不過由於用到太多輔助函示被扣了點分數．

* week5: 本週沒作業

  * 主要是教導一些其他關於集合的處理方式比如說map，filter，flatMap等等相關的內建函示

* week6: anagrams也就是給你一個字串把它重組後顯示所有可能的字

  * 一開始是使用之前用過的pattern match來寫，不過看到提示要用high-order來寫．所以全部重寫花了一點時間．果然就可以一行寫好，不過很難閱讀啊…

  * 整體寫起來比week4 還快，不過也不簡單因為許多high-order的函示要搞懂它的用法與如何好好的用它才能寫得更好． 這次還是被扣分～函示輸出太多記憶體不足....

* week7: 寫出遊戲[Bloxorz](http://www.coolmath-games.com/0-bloxorz/index.html)

  * 總算把最後一個作業都做完了，感覺有點倒吃甘蔗．後面越來越能了解如何去使用high order function 還有使用各種的recursive 來找出解答． 課程上的倒水杯程式真的很有趣，也很有幫助．倒水杯就是給你兩個不容量度的杯子．比如說一個杯子300cc，另外一個500cc 然後請你倒出400cc的水，你能做的就是到滿某杯或是從某杯倒向另外一個杯子．

  * 我從測試四開始的習慣就是在worksheet上面去寫程式．寫好一個馬上測試結果．並且重複思考每一個步驟是不是有問題．這樣得學習方式對於scala不算熟悉的我相當有幫助．

  * 關於這個禮拜的習題，幾個小建議:

    * 原來提供的unit test真的太少，自己寫一些

    * 最後最佳答案找不出來，試著把圖弄小一點找問題

**心得：**

* 學習scala真的讓我有挫折的感覺．不僅僅有許多演算法，更有一些high order function的使用法或是methond/expression parameter ．不過學完之後對於其他語言的學習(swift)我想是有相當程度的幫助．

* Scala的worksheet真的是很酷的東西，而最年輕的語言[Swift](https://developer.apple.com/swift/) 也有playground類似的東西（或者是類似[iPython](http://ipython.org/)的東西)．

* 以前沒有unit test的習慣，由於functional programming的每個函示跟UI的結合比較少而且需要確保每一個部分可以完美的運作，於是我必須要每寫好一個就去思考如何跑unit test．我想這會養成我之後相當好的習慣．

* 我其實很推薦每個程式設計人員都必須要學過functional programming，他能幫助你不斷地把問題切斷成小段的函示，彼此間不會互相干擾，並且能去組合出最後最佳的結果． 可以參考IBM這篇文章: [Functional Thinking](http://www.ibm.com/developerworks/library/j-ft1/)
