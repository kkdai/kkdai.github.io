---
layout: post
title: "[TIL] 中文導讀 - 如何征服老舊的程式碼 (How to conquer legacy code)"
description: ""
category: 
- TodayILearn
tags: ["debug", "TIL", "legacy"]

---

![](https://cdn-images-1.medium.com/max/800/1*Sbh9nL_Q1pr5PcXnIdaicA.png)


## 原文:  How to conquer legacy code

### [鏈結](https://medium.freecodecamp.com/conquer-legacy-code-f9e23a6ab758#.4p11il8lg)

"How to conquer legacy code" 

如何征服老舊的程式碼 (英文):

以下為中文導讀: （原文七分鐘可以讀完)

常常聽到許多人很害怕來解老舊程式碼裡面的 bug 尤其越是剛入門的工程師．甚至有些人會使用到 " 通靈 " 或是 " 猜測 " 來表示閱讀老舊程式碼的痛苦．

這一篇就是一篇很好的教學來教導你如何面對老舊的程式碼．

首先是心態問題，面對這樣的程式碼千萬不要抱著 "這麼不好的架構，我一定可以寫得比他更好的心態來看問題“

It’s easy to look at work that came before you and decide it’s no good and that you can do better. This is the wrong attitude. It will lead you down a very dangerous path.

建議的方式如下：

1. 虛懷若谷看待老舊的程式碼，絕不輕易重寫．
2. 試著畫出循序圖來解釋整個程式碼流程
3. 儘量做最小的變動 (minimum viable change) 來修復問題 
4. 每次的修改讓原來的程式碼能夠更好一點
5. 慢慢地，當你更熟悉之後，就可以仔細思考整個架構與流程上的問題．
6. 想要在老舊程式碼上面增加新功能，千萬要依照著原先的思考流程．
7. 如果有新的架構與流程修改，試著透過 Adapter Pattern 來開始整合．


同場加映: ["The Best Programming Advice I Ever Got" with Rob Pike](http://www.informit.com/articles/article.aspx?p=1941206)

Google 同為 Golang 發明者的 Rob Pike 曾經在某次採訪就有提到，受過最好的 debugging 建議就是來自 Ken Thompthon (C 語言的共同發明者)

"Thinking—without looking at the code—is the best debugging tool of all, because it leads to better software."

最好的除錯工具，不是直接去面對程式碼而是仔細思考整段流程．因為它能夠引導你做出更好的軟體...

## 心得:

還記得當初學校畢業之後，本來還信心滿滿的．結果第一份工作到了視窗軟體開發的公司 (Intervideo Inc.) 就發現所有的環境都不了解之外，還得去面對超過三五年以上的程式碼．

要面對如此龐大的架構的老舊程式碼，當初的部門經理就很推薦我使用循序圖的方式來慢慢瞭解整體架構．透過這些方式進行最小變動的修改來解 bug ，並且慢慢的來了解整個軟體的架構．

後來對於系統架構越來越有了解後，更了解思考對於解決問題的重要性．不論是整體架構的思考，或是對於流程的改造與優化都有事半功倍的效果．

最近常常在推特或是臉書上看到大家對於老舊的程式碼困擾，於是把這篇文章介紹出來，希望能幫到更多的人．