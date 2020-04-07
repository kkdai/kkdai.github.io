---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-03-24 09:25:26+00:00
slug: sometools-for-visual-studio-net
title: SomeTools for Visual Studio .NET
wordpress_id: 217
categories:
- VC++程式設計
---

![Designing .NET Class Libraries: API Usability](http://msdn.microsoft.com/netframework/art/NETFw.jpg)由於工作上的關係，最近有許多機會可以接觸到.NET Framework的相關開發工具與書籍，再此先介紹一本可以了解.NET Framework概觀的書籍([Applied Microsoft .NET Framework Programming](http://www.amazon.com/exec/obidos/tg/detail/-/0735614229/qid=1112118611/sr=8-1/ref=pd_csp_1/104-8049485-0691166?v=glance&s=books&n=507846))。

搜尋網路上的一些相關資料，MSDN也提供了一些[Visual Studio.NET Framework開發上的工具](http://msdn.microsoft.com/library/cht/default.asp?url=/library/CHT/cptools/html/cpconmsildisassemblerildasmexe.asp)，當然如果對於反組譯有興趣可以查看[反組譯的文章](http://www.microsoft.com/taiwan/msdn/columns/DoNet/ToDeoNottoDe.htm)，也可以查看[ILDASM is Your New Best Friend ](http://msdn.microsoft.com/msdnmag/issues/01/05/bugslayer/default.aspx)或是[Ildasm.exe 教學課程](http://msdn.microsoft.com/library/cht/default.asp?url=/library/CHT/cptutorials/html/il_dasm_tutorial.asp)

遠端偵錯方面，檔案在 C:Program FilesMicrosoft Visual Studio .NET 2003Common7PackagesDebugger下可以找到，但是遠端連線軟體，mcvcmon 似乎一定要 mcvcmon.exe -anyuse -tcpip 才能正確執行，詳細可以查看使[用遠端偵錯監視器進行遠端偵錯](http://msdn.microsoft.com/library/cht/default.asp?url=/library/CHT/vsdebug/html/vctskinstallingremotedebugmonitor.asp)。
