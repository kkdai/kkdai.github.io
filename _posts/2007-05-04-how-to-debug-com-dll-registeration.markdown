---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-05-04 03:55:42+00:00
slug: how-to-debug-com-dll-registeration
title: How to debug COM DLL registeration
wordpress_id: 636
categories:
- NET程式設計
---

[![DebugSetting1](http://farm1.static.flickr.com/214/482534716_5c42f1d4aa_o.jpg)](http://www.flickr.com/photos/evanlin/482534716/)

I know many people feel confuse if your COM DLL failed during registeration (using RegSvr32 to register it). I spent a little time, and find out how to debug it. It can help you to find out what is the root cause. OK! let get start.

<blockquote>How can we do if register COM dll failed?
> 
> </blockquote>

Firstable, you can use "dumpbin /dependents YOURDLL.DLL" to find out if you lost some dependency DLL.

Ok! ~~ let go to debug DLL registeration.

  1. Right click you DLL project --> Open "Properties".
  2. Go to "Configuration Properties" --> "Debugging" .
  3. In "Command", fill it "C:WINDOWSsystem32regsvr32.exe"
  4. In "Command Arguments", fill it "$(TargetPath)"
  5. Just press F5 to run it.

Please note, you should setting break point as follows:

  * DllMain
  * DllRegisterServer
  * DllUnregisterServer (if unregister failed).

That's it~~~ 
