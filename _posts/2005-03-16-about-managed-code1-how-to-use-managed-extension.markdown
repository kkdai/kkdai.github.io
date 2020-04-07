---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-03-16 18:00:12+00:00
slug: about-managed-code1-how-to-use-managed-extension
title: About Managed Code(1) — How to use managed extension.
wordpress_id: 213
categories:
- VC++程式設計
---

![Designing .NET Class Libraries: API Usability](http://msdn.microsoft.com/netframework/art/NETFw.jpg)

**"Managed code"** is one of the most important progresses between .Net Framework and VC++. Althouth you can know about how efficiency and how productivity the "managed code" is. but how to proof the efficiency and productivity?

**How efficiency?**   
In this link, [Brad Abrams and Anders Hejlsberg describe the importance of Managed Code](http://msdn.microsoft.com/theshow/Episode035/default.asp). They are taking about lots pregress in managed code, for example: gabage collection, buffer over flow even more you don't have to write addtional destructor for a object, because GC (gabage collection) will help you to automatically destroy this object.

**What's different between Managed code and Native code?  
**The most different between managed code and unmanaged (native) code is "Managed extension". Use managed extension is not a easy thing for me, you can refer [Richard Grimes](http://www.grimes.demon.co.uk/index.htm)'s article in Visual C++ magazine "**Feel at Home with Managed Extensions**" (mm.. I think maybe this article can not find in WWW just check out the old magazine January 2001). This article tell the basically how to use "managed extension", and let you code from native code to managed code. If you can not find this article, maybe you can try his book "[Programming with Managed Extensions for Microsoft Visual C++ .NET](http://www.amazon.com/exec/obidos/tg/detail/-/0735617244/qid=1111132667/sr=8-1/ref=sr_8_xs_ap_i1_xgl14/103-9970321-9491862?v=glance&s=books&n=507846)".

**Want some pratice about manage code?  
**Want a example for how managed code work, or how to make your native code into managed code? I think the Quake2 source code is a good practice to know how managed extension work. You can reference the [CodeProject](http://www.codeproject.com/) article [Quake II .NET](http://www.codeproject.com/managedcpp/Quake2.asp#xx903825xx), they are taking lot of way to port the[ source code](ftp://ftp.idsoftware.com/idstuff/source/quake2.zip) from C into C++ even into managed C++ code. This is a good example to know how they work both in managed or native C++. In this article, they even make installation file to let you test your output executable (real game in quake2).

**How managed C++ to C#?  
**Perhaps you already know what managed c++ is and have already write lots code. But how to let managed C++ code into C#? [Stoyan Damov](http://spaces.msn.com/members/stoyan/) write an article about [.NET Dynamic Software Load Balancing](http://www.codeproject.com/managedcpp/loadbalancing.asp), he also mention about [Some thoughts about MC++ and C#](http://www.codeproject.com/managedcpp/loadbalancing.asp#misc) . This is very useful when we want to transfer out managed C++ into C#.
