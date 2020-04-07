---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-02-15 05:44:51+00:00
slug: c%e6%ba%ab%e6%95%85%e7%9f%a5%e6%96%b0%e9%97%9c%e6%96%bcmacro-inline-%e8%88%87-define
title: '[C++溫故知新]關於Macro, Inline 與 Define'
wordpress_id: 1324
categories:
- C++溫故知新
- 學習文件
---

繼續不斷地學習，不斷的回復自己的記憶






  * #define 在compiler階段就會展開，所以要確認清楚他的展開方式，比如




          #define M1(x)  X*5+3  
          M1(5)*5 ==>  5*5+3*5 =>   (5*5)+(3*5) = 40






  * 他可以不止一行～對於只有一行的if 判斷要小心  if(a)  staement Macro(b) ,  Macro(b) b++; b—; 在此～ b--將會跑不到. 怕失誤要加上 {} 或是改成 inline function


  * 參數有 # 會加上  escape character，也就是  “  —>  ”


  * #define值並不會有值～也就是  




       #define FOO1  
       #ifdef FOO1  
      .. statement1  
      #endif  
       跟  if (FOO1 == 0) 是不同 （其實這樣會compiler error)






  * 如果要#define 一個數字～要換成常數 (constant)     exam: #define FOO 5.3



    * 如果只有一個檔案要用，改成 static const  FOO = 5.3;


    * 多個檔案  extern const FOO= 5.3;


    * CPP裡面  const FOO=5.3;



  * 當專案大的時候，由於 .h 會被很多的檔案載入～所以define macro  不建議放在 .h 容易被其他人用到（或是override)



    * [http://qualitycoding.org/precompiled-headers/](http://qualitycoding.org/precompiled-headers/)



  * 建議寫在.c 或是 .cpp 並且在用完之後馬上  #undef 避免被蓋掉(override)




詳細code整理我放這裡: [https://gist.github.com/kkdai/9014911](https://gist.github.com/kkdai/9014911)




**參考資料:**






  * [http://bbs.nsysu.edu.tw/txtVersion/treasure/program/M.855729774.C/M.948127113.E.html](http://bbs.nsysu.edu.tw/txtVersion/treasure/program/M.855729774.C/M.948127113.E.html)


  * [http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml?showone=Preprocessor_Macros#Preprocessor_Macros](http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml?showone=Preprocessor_Macros#Preprocessor_Macros)


  * [http://openhome.cc/Gossip/CppGossip/inlineFunction.html](http://openhome.cc/Gossip/CppGossip/inlineFunction.html)


  * [http://web2.fg.tp.edu.tw/~cc/blog/wp-content/uploads/2010/06/20100715advanceC.pdf](http://web2.fg.tp.edu.tw/~cc/blog/wp-content/uploads/2010/06/20100715advanceC.pdf)


  * [http://anna-zr.iteye.com/blog/510368](http://anna-zr.iteye.com/blog/510368)


  * Include guard about define [http://en.wikipedia.org/wiki/Include_guard](http://en.wikipedia.org/wiki/Include_guard)


  * [http://qualitycoding.org/precompiled-headers/](http://qualitycoding.org/precompiled-headers/)


  * 9 Code Smells of Preprocessor Use [http://qualitycoding.org/preprocessor/](http://qualitycoding.org/preprocessor/)




 
