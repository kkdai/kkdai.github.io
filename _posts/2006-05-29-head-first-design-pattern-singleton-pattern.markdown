---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-05-29 09:12:41+00:00
slug: head-first-design-pattern-singleton-pattern
title: Head first design pattern–”Singleton pattern”
wordpress_id: 402
categories:
- VC++程式設計
---

[![Head First Design Patterns](http://ec3.images-amazon.com/images/P/0596007124.01._AA240_SCLZZZZZZZ_.jpg)](http://www.amazon.com/gp/product/images/0596007124/ref=dp_image_0/103-7239560-0439836?%5Fencoding=UTF8&n=283155&s=books)  
(Picturefrom [Amazon](http://www.amazon.com/gp/product/0596007124/103-7239560-0439836?v=glance&n=283155))

Sometime you just write code without fully understanding the meaning. Like design pattern, maybe you already know part of them even use them very well. "Head First Design Patterns" is a good book, it use a story to let you know why you must use this pattern (some of us may not know forever).

As this chapter "Singleton pattern", I have already used it in my code everywhere. But I can not get fully understand why we have to use this, how to use this and how to prevent other this in multiple threading programming.

I list a example fot singleton implementation.

<blockquote>  
class CSingleton  
{  
public:  
 static CSingleton * getInstance();  
public:   
 void f1();  
 void f2();  
 void f3();
> 
> private:  
 virtual ~CSingleton();  
 CSingleton();
> 
> private:  
 static CSingleton *m_uniInstance;  
};
> 
> </blockquote>

  

