---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-03-21 07:08:01+00:00
slug: about-metro-app-suspendresume-and-system-sleephibernate
title: About Metro app Suspend/Resume and system sleep/Hibernate
wordpress_id: 1154
categories:
- VC++程式設計
- 下一代視窗系統心得
---

According to [Metro Application Lifetime](http://msdn.microsoft.com/en-us/library/windows/apps/hh464925.aspx) sample and related document, it description about Metro app about suspend/resume.

 

However, I also find I could not make my Metro app enter suspend mode, so I try to figure out some answer from MSFT forum. Refer [here](http://social.msdn.microsoft.com/Forums/en-US/toolsforwinapps/thread/9a6f9c1b-49ea-485b-9f2b-6cce840d84b5/) and [here](http://social.msdn.microsoft.com/Forums/en-US/winappswithnativecode/thread/4ebbc7f1-2f1c-4622-9871-a5f89ff7ef35/?prof=required).

 

Here is my summary for this:

 

  
  1. When leave app about 5 sec or trigger another app about 5 sec, the "suspend" event will called. 
   
  2. "Suspends"/"Resume" event will trigger automatically when you not attach debugger on release build. See "Star without Debugging" on "debug" tab. 
   
  3. If you using debugger, you only trigger "suspend" manually. 
   
  4. Sleep/Hibernate will trigger "Suspend" if your app is release version without debugger attach. 
