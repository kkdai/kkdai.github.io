---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-08-29 20:51:02+00:00
slug: how-to-change-and-find-autoplay-setting
title: How to change and find Autoplay setting?
wordpress_id: 754
categories:
- NET程式設計
- VC++程式設計
- 下一代視窗系統心得
---

![AppPicker.JPG](http://farm2.static.flickr.com/1187/1266023539_717f6f4a02.jpg)  
(Application Picker dialog from XP)

Application Picker is a dialog which will show-up when you insert DVD title or new medium in your computer. It show all playable application and let you to choose one from the list. 

One day, I just woundering where can I find the setting? How could I change the autoplay icon? How could change the autoplay string?

Here is the detail document from MSDN: [Preparing Hardware and Software for Use with AutoPlay](http://msdn2.microsoft.com/en-us/library/aa969331.aspx)

In follows steps, I will show you how to use "REGEDIT.EXE" to find out the setting and how it work.


<!-- more -->
 

  1. In Windows XP command line run "regedit"  
  2. Find the Event handler in:  
[HKEY_LOCAL_MACHINESOFTWAREMicrosoftWindowsCurrentVersionExplorerAutoplayHandlersEventHandlers]  

  3. The Event handler show the all kind event which come from Windows, find out "PlayDVDMovieOnArrival". This is all application picker list when you insert DVD movie title into your computer.  
  
[HKEY_LOCAL_MACHINESOFTWAREMicrosoftWindowsCurrentVersionExplorerAutoplayHandlersEventHandlersPlayDVDMovieOnArrival]  
"MSOpenFolder"="" _ // For Open folder_  
"RPPlayDVDMovieOnArrival"="" _//Use Real play to playback movie_  
"MPCPlayDVDMovieOnArrival"="" _//Use media player to playback movie._  

  4. Ok this is the first step.Then we will go to "Handlers" to figure out how it work and what application it used.  
[HKEY_LOCAL_MACHINESOFTWAREMicrosoftWindowsCurrentVersionExplorerAutoplayHandlersHandlers]  

  5. Every item in "Handlers" is presented a application. Let me show Media Player for your reference:  
[HKEY_LOCAL_MACHINESOFTWAREMicrosoftWindowsCurrentVersionExplorerAutoplayHandlersHandlersMSPlayDVDMovieOnArrival]  
"Action"="@wmploc.dll,-6504" _//Will show in dialog "Play DVD Movie"  
_"Provider"="@wmploc.dll,-6502" _//Provider name "Using Windows Media Player"_  
"InvokeProgID"="WMP.DVD" _//Application ID which you can find in GUID.  
_"InvokeVerb"="play" _//Playback command  
_"DefaultIcon"=hex(2):25,00,50,00,72,00,6f,00,67,00,72,00,61,00,6d,00,46,00,69,  
00,6c,00,65,00,73,00,25,00,5c,00,57,00,69,00,6e,00,64,00,6f,00,77,00,73,00,  
20,00,4d,00,65,00,64,00,69,00,61,00,20,00,50,00,6c,00,61,00,79,00,65,00,72,  
00,5c,00,77,00,6d,00,70,00,6c,00,61,00,79,00,65,00,72,00,2e,00,65,00,78,00,  
65,00,2c,00,30,00,00,00  
  6. If you want to change any of it, just do it. Before you change anything, please remember to backup your registry setting.
