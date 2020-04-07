---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-08-15 10:20:27+00:00
slug: unresolved-external-symbol-wchar_t-__stdcall-_com_utilconvertstringtobstr
title: unresolved external symbol “wchar_t * __stdcall _com_util::ConvertStringToBSTR
wordpress_id: 464
categories:
- NET程式設計
---

![](http://msdn.microsoft.com/vstudio/images/VS2005_logo_product_home.gif)

If you link code found some error like bellow:

<blockquote>error LNK2019: unresolved external symbol "wchar_t * __stdcall _com_util::ConvertStringToBSTR(char const *)" (?ConvertStringToBSTR@_com_util@@YGPA_WPBD@Z) referenced in function "public: __thiscall _bstr_t::Data_t::Data_t(char const *)" ([??0Data_t@_bstr_t@@QAE@PBD@Z](mailto:??0Data_t@_bstr_t@@QAE@PBD@Z))
> 
> </blockquote>

**Solve:**

Try to link "comsuppwd.lib" in debug AdditionalDependencies. Link "comsuppw.lib" in release AdditionalDependencies.

**Reference:**

  1. [http://msdn2.microsoft.com/en-us/library/t58y75c0.aspx](http://msdn2.microsoft.com/en-us/library/t58y75c0.aspx)

  2. [http://www3.ccw.com.cn/club/essence/200103/532.htm](http://www3.ccw.com.cn/club/essence/200103/532.htm)
