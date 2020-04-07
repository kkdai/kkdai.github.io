---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-10-08 22:52:27+00:00
slug: quicktimeqts-%e5%b0%8e%e8%87%b4%e7%9a%84%e7%a8%8b%e5%bc%8f%e5%9f%b7%e8%a1%8c%e9%8c%af%e8%aa%a4%ef%bc%8c%e4%b8%a6%e4%b8%94%e7%84%a1%e6%b3%95%e9%a0%86%e5%88%a9%e7%a7%bb%e9%99%a4quicktime-quicktimeqts
title: QuickTime.qts 導致的程式執行錯誤，並且無法順利移除Quicktime (QuickTime.qts cause app crash and
  could not uninstall quicktime)
wordpress_id: 967
categories:
- VC++程式設計
- 學習文件
---

![Inside QuickTime](http://images.apple.com/quicktime/images/index_sidebar_insideqt.jpg)

最近在做結婚光碟的時候，發現製作光碟的UVS(Ulead Video Studio) 一直無法正常的執行。由於我自己就是做這行的，於是直接下去看整個crash call stack。發現CRASH 在一開始~~~整個在initialized filter 的時候死在 (Quicktime.qts)。

經過許多文章裡面的尋找

  * ##### [YouTube - Re: _Quicktime.qts_ error in Macromedia Projector](http://www.youtube.com/watch?v=x8EM1PSbJlY)

  * ##### [QuickTime 7](http://tw.creative.com/support/quicktime7/)

這幾篇的文章都提到~ 要先將Quicktime.qts 移除，我就直接把 C:WindowsSystem32Quicktime.qts 移除

結果發現無法改善~~~~ 並且我無法順利的移除Quicktime(Uninstall Quicktime)

後來我發現~~ 因為我有裝一套Storm player 而這次有問題的Quicktime.qts 是在 那個目錄裡面的~~~~~

所以我想到正確的解法是~~~

  1. 尋找Quicktime.qts (Find out all Quicktime.qts)  
  2. 將它改名字 (Rename it)  
  3. 移除 Quicktime (uninstall quicktime)  
  4. 重新安裝一次 (reinstall again)

這樣果然就成功了!!!!!
