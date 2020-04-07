---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-07-17 04:21:35+00:00
slug: solve-cant-not-install-vc6-and-sp5-under-vista
title: Solve can’t not install VC6 and SP5 under Vista.
wordpress_id: 444
categories:
- 下一代視窗系統心得
---

![](http://www.windows-vista-info.com/images/pictures/windows-vista-logo-1.jpg)

Please refer the article (Re: Visual Studio 6.0 SP5 setup on Vista Beta 2 (5384)? )  
[http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=518043&SiteID=1](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=518043&SiteID=1)


<!-- more -->


If you want install VC6 SP5 on Vista,

save the attached file into your SP5 setup directory to replace SP598ENT.STF

then install it.

[Download file](http://www.evanlin.com/blog/archives/20060719/SP598ENT.STF)




* * *

Answer Re: Visual Studio 6.0 SP5 setup on Vista Beta 2 (5384)? 

So I know there was the whole 'copy your files over from XP' but i wasn't satisfied with that, so I did some digging, and figured out ... it checks for mdac which makes it fail, but we're going to remove that check for mdac.

how to make service pack 5 install on vista beta 2 without another pc!

Tutorial:  
Step 1) Open C:ServicePack5Dirsp598ent.stf with 'Notepad.exe'  
Step 2) Replace the following line

13 Group 28 36 38 29 30 32 26 27 14 25 16 17 20 18 19 15 39 21 22 24 23 43

-with-

13 Group 28 38 29 30 32 26 27 14 25 16 17 20 18 19 15 39 21 22 24 23 43 

Step 3) Delete the following lines leaving only a carriage return where it was

36 Depend "27 ? : 37"   
37 IsWin95 CustomAction "sp598ent.dll,CheckForMDAC"

Step 4) Save and close C:ServicePack5Dirsp598ent.stf  
Step 5) Run setupsp5.exe

I would offer a download link to the fixed file, but I believe that may be a copyright violation. 

guideX

* * *
