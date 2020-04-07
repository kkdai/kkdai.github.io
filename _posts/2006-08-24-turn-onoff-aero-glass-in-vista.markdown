---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-08-24 12:29:26+00:00
slug: turn-onoff-aero-glass-in-vista
title: Turn on/off  “Aero glass” in Vista
wordpress_id: 470
categories:
- 下一代視窗系統心得
---

![Windows Vista Aero Glass](http://www.jimmah.com/Vista/imgs/aero_glass.PNG)

"Aero Glass" is a new feature which allows Vista to show aero effect like the snapshot. 

**How to turn on or turn off it?  
**[My Computer]--> [Advance Setting] --> [Performance Setting] --> [Video Effect] --> [Enable Desktop Composition].

  
Or[Ctrl+ Shift+ F9]  
  
**How to turn on/off it by Window API?**[  
DwmEnableComposition(BOOL fEnable);](http://windowssdk.msdn.microsoft.com/en-us/library/ms647255.aspx)

**How to use "Aero Glass" to blur a Window?  
**[DwmEnableBlurBehindWindow ](http://windowssdk.msdn.microsoft.com/en-us/library/ms647254.aspx) and [DwmExtendFrameIntoClientArea](http://windowssdk.msdn.microsoft.com/en-us/library/ms647257.aspx).

**  
For more detail information to refer **

  * [使用VB6.0在Windows Vista下全磨砂玻璃窗口](http://www.mndsoft.com/blog/article.asp?id=715)
