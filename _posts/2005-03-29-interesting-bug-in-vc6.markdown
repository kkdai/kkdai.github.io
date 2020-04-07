---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-03-29 10:27:34+00:00
slug: interesting-bug-in-vc6
title: Interesting bug in VC6
wordpress_id: 218
categories:
- VC++程式設計
---

[![Route 64 - Kang Su Gatlin talks about 64-bit](http://msdn.microsoft.com/nodehomes/graphics/80x60/Channel9_80x60.jpg)](http://channel9.msdn.com/showpost.aspx?postid=51671)最近在做VC6轉換VC7的時候，倒是慢慢發現VC6的一些bug，舉例來說，看一下下面的範例程式:

<blockquote> if (int i)  
 {  
    i = 1;  
 }
> 
> </blockquote>

在VC6只會得到一個warnning:

<blockquote>warning C4700: local variable 'i' used without having been initialized
> 
> </blockquote>

但是由於Constructor 通常不會有回傳值(return value)，所以基本上在if() 不應該有變數的起始。所以這樣的code在GNU C++會發現是無法compiler過的~~~

當然VC7 修掉了這個嚴重的bug，你可以看到出現:

<blockquote>error C2059: syntax error : ')'  

> 
> </blockquote>

蠻有趣的bug，不過~~~ 若有人這樣寫code 可就慘了，因為雖然constructor沒有回傳值(return value)，但是在VC6中， if (int i)卻是判斷為true~~~ 這樣~~可能是與原來的想法會有點出入~~~

所以有時候利用VC7來compiler一下現在的code，也是一個好主意。
