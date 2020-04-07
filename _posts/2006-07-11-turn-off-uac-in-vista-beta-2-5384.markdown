---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-07-11 05:59:37+00:00
slug: turn-off-uac-in-vista-beta-2-5384
title: 'Turn off UAC in VISTA beta 2 #5384.'
wordpress_id: 439
categories:
- 下一代視窗系統心得
---

![](http://www.windows-vista-info.com/images/pictures/windows-vista-logo-1.jpg)

[UAC (User Account Control)](http://www.microsoft.com/technet/windowsvista/security/uac.mspx) is a new architecture under Vista which end users can run as standard users (not administrators) and still be productive. It got a little complicated to register DLL by RegSvr32.exe. 

  * When register your DLL via RegSvr32.exe it will failed by error code 0x8002801c and 0x80070005. 
  * When you use "Administrator" to install component via installation  APP, this situation does not happen during Installation.

If you want to solve this problem, just turn off [UAC (User Account Control)](http://www.microsoft.com/technet/windowsvista/security/uac.mspx).

This article will show you how to turn off [UAC (User Account Control)](http://www.microsoft.com/technet/windowsvista/security/uac.mspx) in Vista beta2. (It should be noted, [some article(UACBlog) ](http://blogs.msdn.com/uac/)also tell us not cancel [UAC](http://www.microsoft.com/technet/windowsvista/security/uac.mspx) ).

  1. In traditional view, [Settings] --> [Console Panel]
  2. Press [User Account] --> [Change security settings].
  3. It will pop up a dialog to show [UAC](http://www.microsoft.com/technet/windowsvista/security/uac.mspx). Just turn off it.

That's it~~~
