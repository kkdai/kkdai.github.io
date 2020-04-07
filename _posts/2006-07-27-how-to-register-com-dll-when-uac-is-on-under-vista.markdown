---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-07-27 10:11:58+00:00
slug: how-to-register-com-dll-when-uac-is-on-under-vista
title: How to register COM DLL when UAC is on under vista
wordpress_id: 454
categories:
- 下一代視窗系統心得
---

[![](http://images.google.com.tw/images?q=tbn:eJhTLW4R6S6viM:www.kiruthik.com/content/binary/Windows_Vista_logo.jpg)](http://images.google.com.tw/imgres?imgurl=http://www.kiruthik.com/content/binary/Windows_Vista_logo.jpg&imgrefurl=http://www.kiruthik.com/CategoryView,category,Technology.aspx&h=269&w=367&sz=16&hl=zh-TW&start=1&tbnid=eJhTLW4R6S6viM:&tbnh=89&tbnw=122&prev=/images%3Fq%3Dvista%2Blogo%26svnum%3D10%26hl%3Dzh-TW%26lr%3D)

[UAC (User Account Control)](http://www.microsoft.com/technet/windowsvista/security/uac.mspx)  is complicate as [I disussed before](http://www.evanlin.com/blog/archives/000532.html). Even you are a "Administrators" (Yes~~ it has 's', in Vista you only have one "Administrator" but much "Administrators"), you can not register the COM component. There is a way to tell you how to work it out!!

  1. Write a batch file "regsvr32 foo.dll" and use right click "Run as administrator".
  2. Open Programs-->Accessories-->Command Prompt.   (right click "Run as administrator".) 

Such you can register your COM component, or you can use "Administrator" account (it will be disable by default, just go to "Manage-->Local Users and Groups--> Users" to open it.
