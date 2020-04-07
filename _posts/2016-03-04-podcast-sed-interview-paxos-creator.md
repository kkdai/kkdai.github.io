---
layout: post
title: "Paxos 發明者 Leslie Lamport 的採訪的 Podcast"
description: ""
category: 
- 網路上好玩的事情
tags: ["paxos", "distribution system"]

---

[![](http://softwareengineeringdaily.com/wp-content/uploads/2015/08/sed_logo_updated.png)
](http://softwareengineeringdaily.com/2016/02/26/distributed-systems-with-leslie-lamport/)

## [線上收聽的鏈結](http://softwareengineeringdaily.com/2016/02/26/distributed-systems-with-leslie-lamport/)



![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Leslie_Lamport.jpg/220px-Leslie_Lamport.jpg)


### 起源:

最近有在聽Podcast的習慣，加上我一直有學習分散式系統的部分(尤其是Paxos跟Raft)．於是試著上去iTune來搜尋，竟然有找到Paxos作者Leslie Lamport受訪的podcast．


### 為何會做Paxos

裡面相當的有趣，除了有講到當初會做Paxos是因為一開始Lamport在做他的[Time Clock](http://research.microsoft.com/en-us/um/people/lamport/pubs/time-clocks.pdf)論文的時候，正個論文的架構是在討論如何避免失敗．但是一個有趣的問題來了，如果真正的遇到失敗要如何去恢復與處理．當初在研究Time Clock也是主要專注在分散式資料庫的方面，但是Lamport發現其實可以適用到任何的分散式系統，於是就慢慢演化成Paxos．

![](http://softwareengineeringdaily.com/wp-content/uploads/2016/02/main_paxos.jpg)

### Paxos發表經過
就如大家所熟知的，由於Lamport在當時相當喜歡透過哲學的方式來敘述事情．也就是大家所熟知的[Paxos議會故事](http://research.microsoft.com/users/lamport/pubs/lamport-paxos.pdf)，以至於Paxos論文一出來，大家完全看不懂也就不注意，就這樣被擱置了好幾年(據說十年)．根據Lamport說，他認識的人也只有一個人聽得懂Paxos並且有幫助他宣傳，所以他又出了另外一篇廣為人知的[Paxos Make Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)

### 最近的談話"Thinging For Programmer"

後來主持人又跟Lamport提到2014在[MSFT Build "Thinking For Programmers](https://www.youtube.com/watch?v=4nhFqf_46ZQ)的演說．裡面重要的概念就是每個程式設計師應該要先將你要寫的系統在腦中整個思考過，這邊講的思考建議是透過數學的方式來思考，因為這樣是最嚴謹的．透過嚴謹的思考，才能夠將整個系統的瓶頸跟問題找出來並且可以再發現之前來解決．

Lamport也提出一個範例，假設你要寫一個排序的程式．如果你沒有仔細思考要用哪種排序的方式而使用了Bubble Sort，不論你使用哪種程式語言，你使用哪種framework，你所建置的系統都不會快．因為他有先天性的限制，侷限了你的最佳速度，而這個是透過程式語言本身無法解決的．所以Lamport建議我們應該要先專注於"How should you do" 而不是 "What should you do"． 這邊提的"How" 代表你要用哪些方式，使用哪些演算法來解決問題． "What"才是你要用哪些語言來實現它．

最後也有提到UML或是透過繪圖方式來思考，不是不好．但是Lamport認為那些方式不容易清楚(precisely)而嚴謹的描述，容易造成混淆的區域．這樣就無法清楚的表達而思考整個系統的來龍去脈．

主持人提到會不會有人數學不好．而改用繪圖來思考？ Lamport認為身為程式設計師，要清楚的頭腦來思考是絕對的重要．而透過清楚而精準的表達方式來呈現自己的思考脈絡更是一個程式設計師基本的學習．他很建議使用數學得方式來表達．

### 心得

想不到Leslie Lamport接受訪問的時候用字遣詞很容易了解．讓我們可以很了解這位大師的整個思考脈絡．這篇採訪實在相當的精彩，不論對於思考問題與解決問題的方法論．都有相當的建議．

## 相關鏈結
- [第一版的Paxos](http://research.microsoft.com/users/lamport/pubs/lamport-paxos.pdf)
- [Paxos Make Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)
- [Leslie Lamport在2014的MSFT Build "Thinking For Programmers](https://www.youtube.com/watch?v=4nhFqf_46ZQ)
- [之前寫的Paxos學習心得](http://www.evanlin.com/mit6824-week3-paxos/)