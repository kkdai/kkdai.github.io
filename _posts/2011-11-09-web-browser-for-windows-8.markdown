---
layout: post
layout: post
author: kkdai
comments: true
date: 2011-11-09 08:10:00+00:00
slug: web-browser-for-windows-8
title: Web browser for Windows 8
wordpress_id: 1135
categories:
- 學習文件
- 下一代視窗系統心得
---

![](http://files.channel9.msdn.com/thumbnail/ace11650-7aa0-4a31-8689-0890e91146e9.png)

It need note that Windows 8 has two version of IE (it should say, one application but two entry for different mode)



	
  * Desktop IE

	
  * Metro IE


But it is very tricky, if you type “open [http://Web](http://Web)” on desktop explorer will open “**Metro IE**”

When you try to use this command to open web side, please note metro browser will be default option, not desktop browser.

ShellExecute(NULL,TEXT("open"), TEXT("http://msdn.microsoft.com"), TEXT(""),NULL,SW_SHOWNORMAL);

To specific using desktop browser, it should specific command on [HKEY_CLASSES_ROOThttpshellopencommand]
