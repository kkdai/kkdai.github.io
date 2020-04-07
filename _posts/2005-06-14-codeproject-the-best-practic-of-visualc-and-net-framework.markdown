---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-06-14 17:45:35+00:00
slug: codeproject-the-best-practic-of-visualc-and-net-framework
title: “CodeProject” the best practic of VisualC++ and .NET framework
wordpress_id: 244
categories:
- VC++程式設計
---

[![The Code Project](http://www.codeproject.com/images/standard/logo225x72.gif)](http://www.codeproject.com/)

說起[CodeProject](http://www.codeproject.com/)，大概學習VC++與.NET的人都不會不知道的一個好網站。裡面有個蠻好用的專案[CTreeProperty](http://www.codeproject.com/property/treepropsheet.asp)算是相當好使用的專案之ㄧ，可以簡單秀出如下圖一樣的Dialog.

![Windows XP - Aqua - screenshot](http://www.codeproject.com/property/TreePropSheet/Aqua.png)

(PIC: [CTreePropSheet - A Netscape/Visual Studio .NET like Preferences Dialog](http://www.codeproject.com/property/treepropsheet.asp))

讓我困擾許久的是~~如何搞出Minimize Button 呢?[後來找到答案了](http://www.codeproject.com/property/treepropsheet.asp?df=100&forumid=14680&fr=26#xx917856xx)

<blockquote>BOOL CTreePropSheet::OnInitDialog()  
{  
......  
  
::SetWindowLong( m_hWnd, GWL_STYLE, GetStyle() | WS_MINIMIZEBOX );  
GetSystemMenu( FALSE )->InsertMenu( -1, MF_BYPOSITION | MF_STRING, SC_ICON, "Minimize" );  
DrawMenuBar();  
}   

> 
> </blockquote>

感謝need_help 提供
