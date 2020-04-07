---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-03-29 13:10:43+00:00
slug: the-scope-bug-of-for-loop
title: The scope bug of “for loop”
wordpress_id: 219
categories:
- STL學習心得
---

[![Route 64 - Kang Su Gatlin talks about 64-bit](http://msdn.microsoft.com/nodehomes/graphics/80x60/Channel9_80x60.jpg)](http://channel9.msdn.com/showpost.aspx?postid=51671)對於for loop的使用，相信大家跟我一樣，總是習慣去撰寫

<blockquote>for(int i=0; i<5; ++i)  
{
> 
> .......
> 
> }
> 
> cout << i ;
> 
> </blockquote>

但是，若是根據以上的程式，利用VC6或是BCB會看到哪樣的ouput i?很簡單~~就是 i = 5，但是正常來說，i 的scope 在哪裡??

i 應該是在for 之內的，所以他的生命週期(scope)應該也是在其中，但是由於VC++與BCB的錯誤，此得i 的scope 變成了for 之外，造成許多程式的問題。

**解決方式:** #define for   if(0); else for

**參考網站:**[ BBS文章，參照侯捷老師上課內容](http://bbs.ice.cycu.edu.tw/cgi-bin/bbscon?board=CS.Language&file=M.1053081043.A&num=171)
