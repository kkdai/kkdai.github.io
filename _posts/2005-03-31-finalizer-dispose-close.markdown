---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-03-31 13:54:20+00:00
slug: finalizer-dispose-close
title: Finalizer, Dispose, Close
wordpress_id: 220
categories:
- VC++程式設計
---

![Designing .NET Class Libraries: Designing for a Managed Memory World](http://msdn.microsoft.com/netframework/art/NETFw.jpg)[Designing .NET Class Libraries](http://msdn.microsoft.com/netframework/programming/classlibraries/default.aspx) is very good place that I am used to go here and study something anout .NET Framework. I also like [Brad Abrams](http://blogs.msdn.com/brada/) very much, because hi usually use very simple example to explain such hard thing like "dispose", "finalizer", "Manage code", "CLR" ... . In the speech[Designing for a Managed Memory World](http://msdn.microsoft.com/netframework/programming/classlibraries/managedmemory), [Brad Abrams](http://blogs.msdn.com/brada/) focus on **"Gabage Colloection"**, and talking about _finalizer_ (seems he like this word more than destructor)** **, _dispose_( I think dispose is new concept in .NET framework, but it really hard to use it well).

He also talk about _try {}_ ,_excption{}_ and_ finally {}_ to manage the memory well. Nested use _try{} finally {}_ can make exception more clear and can handle more condition even we don't think it will happen.

<blockquote>static void CopyFile(file sourcdeFile, file destFile){
> 
> try {
> 
>     OpensourceFile(sourcdeFile);
> 
>    try { OpensourceFile(destFile);  }
> 
>    finally { CloseFile(destFile); }
> 
>    }
> 
>  finally { CloseFile(sourcdeFile); }
> 
> }
> 
> </blockquote>

**Important concept about GC (Gabage Collection) is when dispose, close, finalize call?** _The anwser is use code to call **dispose** and **close**; but GC will call **finalize** when this object should be collection back._
