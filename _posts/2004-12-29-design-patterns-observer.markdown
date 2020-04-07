---
layout: post
layout: post
author: kkdai
comments: true
date: 2004-12-29 09:02:56+00:00
slug: design-patterns-observer
title: Design   Patterns–Observer
wordpress_id: 175
categories:
- 學習文件
---

![](http://sern.ucalgary.ca/courses/SENG/609.04/W98/lamsh/observer.gif)

**使用時機:**

可以建立出關於"訂閱與發佈"的系統(一對一、一對多)，在"訂閱與發佈"之中，你可以不在乎有幾個人的訂閱，並且一次針對所有的人去發佈。

**可能問題:**

不論是在Design Patterns的書上、或是在網站的資料上，都會提到必須"確保subject 在知會他人之前是完好無缺的"(**Making sure subject state is self-consistent before notification**. )，但是在實作上會有相當大的困難。

_舉例而言:_小明一家三口都訂閱"貧"果日報，送報順序是:爸->媽->小明。有一天，爸爸看到上面的新聞報導著火山爆發的消息，所以一家三口都搬家，造成報紙無法送至其他人手中，造成問題。

也就是Observer在接到Notify後，自身所反映的update。某些情況下，不僅僅將自己的Observer砍掉(detach)，也將其他的Observer都移除，造成subject之後的Notify會產生問題(雖然說Observer彼此間不應該存在太緊密地關係，也就是不該知道彼此間是否有attach 某些subject)但是這樣的實作確實容易存在，也不易解決，一般解決方式會採取的是:

_Subject在執行Noyify前，對於每個Observer均增加一次Attach，避免 Observer自身的update去驅動其他Observer的Dettach 而失去Subject與Observer之間的聯繫，(如果是COM，將造成無法執行某些Interface進而Access Violation。)_

**心得:**

這樣的Pattern實在相當好用，但是這個CASE主要是一對多，若是單純的一對一的更新，可透過傳入函式或是某個程式指標的方式去驅動即可。詳細的Implement可以參考書上及網路上的應用，在此僅對於此Pattern用法與相關問題作一個討論。

**Reference:**

  * [SENG 609.04 Design Pattern "Observer Design Pattern"](http://sern.ucalgary.ca/courses/SENG/609.04/W98/lamsh/observerLib.html)
  * [Observer Pattern](http://c2.com/cgi/wiki?ObserverPattern)
  * [Design Patterns Links](http://hem.passagen.se/gumby/cs/patterns.html)
  * [On Using the Observer Design Pattern](http://www.wohnklo.de/patterns/observer.html)
