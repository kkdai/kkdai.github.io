---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-03-10 07:37:03+00:00
slug: naming-convensions
title: Naming convensions
wordpress_id: 208
categories:
- VC++程式設計
---

![Designing .NET Class Libraries: API Usability](http://msdn.microsoft.com/netframework/art/NETFw.jpg)

Naming is alway difficult thing for me. But when look the speech which [Brad Abrams ](http://blogs.msdn.com/brada/) talking about[ naming convension](http://msdn.microsoft.com/netframework/programming/classlibraries/namingconventions/)， I think he provide more detail situation what we have to do when we need naming our variables. Some interesting about this speech as bellow:

  * **"Google test":** he just talk about _avoid abbreviation_. He say you can use google to test the abbrebiation.
  * **About casing rule**: Just use camelCase in parameter naming,otherwise use PascalCase. (I think this rule give me some help)
  * **Do not use underscore**:  Do not use it when naming. Just like Some_Variable~~
  * **Don't use Hungarian Notation**: [Brad Abrams ](http://blogs.msdn.com/brada/)also talk about although Hungarian Notation is usefull in the past, but according the IDE progress more and more. Microsoft already ban their engineer using Hungarian Notation. So~~ in the public API do not use it, just use it in your function implementation(if you still want to use it).

There still have some great speech in the [Designing .NET Class Libraries](http://msdn.microsoft.com/netframework/programming/classlibraries/default.aspx). You can choose what you interest topic and watch it online.
