---
layout: post
layout: post
author: kkdai
comments: true
date: 2004-03-29 13:16:09+00:00
slug: unix%e4%b8%8a%e8%87%aa%e5%8b%95%e7%94%a2%e7%94%9f%e7%9a%84installshiled%ef%bd%9e%ef%bd%9eautomake
title: Unix上自動產生的InstallShiled～～Automake
wordpress_id: 66
categories:
- 學習文件
---

![為了簡易流程而學的Automake](http://www.evanlin.com/blog/archives/0329/automake.gif)
 之前去[友立資訊](http://www.ulead.com.tw)面試的時候，聽到他們都是利用[Automake](http://www.gnu.org/software/automake/automake.html)來製造Makefile來編譯在UNIX上面的程式語言設計，他們問我會不會使用這套軟體的時候。由於在設計與BBS相關的程式部分，都是修改程式碼與新增部分程式區段的程式碼，所以幾乎都僅僅只是修改makefile來執行罷了，並沒有必要到說去自行設計一個Makefile的撰寫，即使之前寫一些簡單的Multi-processes的程式也僅僅是直接手寫一些簡單的Makefile，於是在我心中決定必須把這套軟體好好的學學，相信搭配[之前所提過的Global](http://www.evanlin.com/blog/archives/000061.html)來建制程式碼參考用網頁之後，可以更方便的使用[Gdb](http://www.gnu.org/software/gdb/gdb.html)來作debug的工作，當然對於這個陌生的軟體，又是一個向[Google](http://www.google.com)請教的好時間，於是我找到一篇不錯的[Automake參考文獻(由陳雍穆所寫）](http://netlab.cse.yzu.edu.tw/~armor/columns/automake/automake.htm)，透過這位大哥詳細的介紹，其實很快的就會使用了[Automake](http://www.gnu.org/software/automake/automake.html)來製造屬於自己的Makefile檔


　


<!-- more -->


**AutoMake簡易使用法**




1.利用Autoscan 來掃瞄出整個目錄的檔案




2.將編輯出來的configure.scan改為configure.in，並將檔案中的AC_OUTPUT()
-->AC_OUTPUT(Makefile)




3.執行 aclocal 和 autoconf ，分別會產生 aclocal.m4 及 configure 兩個檔案




4.利用文字編輯器，自行編輯Makefile.am(此時尚無此檔，請自行編輯），在此注意您所使用的原始檔必須為*.c




AUTOMAKE_OPTIONS= foreign
bin_PROGRAMS= 程式執行檔名稱
程式執行檔名稱_SOURCES= （裡面放相關的C與H） 




5.使用 automake --add-missing 將 Makefile.in
產生出來，這樣一來就可以正常運作 
