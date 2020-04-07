---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-01-18 06:53:00+00:00
slug: resolve-some-build-error-in-vs2005
title: Resolve some build error in VS2005
wordpress_id: 328
categories:
- VC++程式設計
---

![Designing .NET Class Libraries: API Usability](http://msdn.microsoft.com/netframework/art/NETFw.jpg)

  1. ****
  2. ****
  3. ****

****

  1. **Remove DirectX SDK  
**Because VS2005 doesn't need DirectX SDK, so ~~~ if you use some library from DirectX. Please use   
  
#if (defined(_MSC_VER) && (_MSC_VER > 1310)) //For VS2005 Compiler  
///.....  
#endif  
  
Which the version of compiler VS2003 is 1310, and VS2005 is 1400.   
  

  2. **Resolve "ambiguous symbol" build error from IXMLDOMElementPtr  
**From Microsoft PRB [316317(PRB: Compiler Errors When You Use #import with XML in Visual C++ .NET)](http://support.microsoft.com/default.aspx?scid=kb;en-us;316317) and [26914(PRB: Compiler Errors When You Use #import with XML)](http://support.microsoft.com/default.aspx?scid=kb;EN-US;269194) .  
  
We may use more precise directory path to import msxml.dll, such like import "C:MSXMLmsxml.dll".  
  
Also refer [Belarce's post on CodeGuru](http://www.codeguru.com/forum/showthread.php?t=264620&page=1&pp=15). You can download the old version msxml.h and include it before you import <msxml.dll>
