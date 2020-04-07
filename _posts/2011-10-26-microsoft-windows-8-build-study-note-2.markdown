---
layout: post
layout: post
author: kkdai
comments: true
date: 2011-10-26 11:15:23+00:00
slug: microsoft-windows-8-build-study-note-2
title: MICROSOFT WINDOWS 8 BUILD STUDY NOTE (2)
wordpress_id: 1133
categories:
- 學習文件
- 下一代視窗系統心得
---

![](http://files.channel9.msdn.com/thumbnail/ace11650-7aa0-4a31-8689-0890e91146e9.png)

**APP-737T - Metro style apps using XAML - what you need to know**

Refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-737T](http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-737T)

This session talking about XAML for Metro style application implement. Focus on XAML for C/C++/C#/VB on Metro style application development.



	
  1. Consistent with WPF and silverlight

	
  2. New in Windows 8 Metro

	
    1. New look and support for touch

	
    2. Deployment by Windows Store

	
    3. Tile –> splash screen to application




	
  3. New XAML UI Control

	
    1. Build in “Grouping”

	
    2. Windows 8 look and feel and its “selection model”

	
    3. Media player on Metro Style

	
    4. Grid view and Flip view

	
    5. Application Bar: Swipe from bottom/top to display

	
    6. Manipulation and gestures: It could handle rotation on Button_Clicp event.




	
  4. Metro Style App Concept

	
    1. Diversity of display and resolution

	
    2. Layout changed

	
      1. Snapped(small one on split), Filled (big one on split) and Full Screen

	
      2. It could be tested on simulator on VS2011 CTP.







	
  5. Metro style app lifetime

	
    1. Background apps are “Suspended”

	
      1. App notified.




	
    2. App will terminated when memory is low

	
      1. App will not notified.




	
    3. Event:

	
      1. Application.Current.Suspending –> Save state when suspend

	
      2. Application.Current.Resuming –> Restore state when resume








**APP-116T - Prepare your apps for Windows 8 and beyond**

Refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-116T](http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-116T)

This is talking about application compatibility on Windows 8.



	
  1. Windows 8 is compatible with apps that run on Windows 7

	
  2. OS version increasement

	
    1. Application should not set upper bound for version handle

	
    2. Windows 8 version is 6.2




	
  3. DWM (Desktop Windows Manager) always ON

	
    1. If application try to turn off it, it will failed silently.

	
    2. Color depth must on 32-bit (for DWM limitation) but lower could be simulated




	
  4. Startuo application changed

	
    1. Apps on “Start” should goes to “task scheduler” or “automatic maintance”




	
  5. .NET 3.5 demanded

	
    1. Default is .NET 4.5 it could enable if user install .NET 3.5 application.




	
  6. Use Windows Resource Widely

	
    1. Use RegExpandSZ for path query

	
    2. New error code add one

	
      1. Some SUCCESSED might goes to FAILED.







	
  7. Manifest your executable

	
    1. Need add DPIAWARE for your application.

	
    2. Application compatibility <compatibility xmln..>

	
      1. Starting on Win7, if no this tag **treat it as Vista application**.




	
    3. Metro Style Application should managed it manifest

	
      1. <OSMinVersion><OSMaxVersionTested> to manage your metro app.








Actually, [Windows CTP Compatibility Cookbook](http://www.microsoft.com/download/en/details.aspx?id=27416) provide more overview for this.

**APP-123T - Enabling trials and in-app offers in your Metro style app
**refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-123T](http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-123T)



	
  1. Trail is really matter

	
    1. 70 times download, 10 times revenue and 10% conversion.




	
  2. In-App Offer

	
    1. 72% revenue comes from app which present “In-App offers”

	
    2. 48% comes from “In-App offers”




	
  3. Flxibility option for this.

	
    1. Use your existing commerce

	
      1. maintain relationship, subscription and consumable phuchases.




	
    2. Ad supporteds

	
      1. Choice of ad controls




	
    3. One time purchase

	
      1. Time limited trail and features differentiated trails.




	
    4. Purchase over time

	
      1. Persisent purchase and Expiring purchase.







	
  4. Implement basic

	
    1. Check License

	
      1. It could use simulate since store still no open




	
    2. Get latest listing data

	
    3. Prompt for purchase





**PLAT-775T/**PLAT-776T** - Your Metro style app, video and audio, Part 1/Part 2**

refer:  [http://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-775T](http://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-775T)
refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-776T](http://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-776T)

It discussion about media API and Media element:



	
  1. Using media API could easy to build up tailored user experience application with little domain knowledge.

	
  2. HTML 5 standard <audio><video>

	
    1. DXVA full support

	
    2. You can creat rich media by CSS, JS and DOM.




	
  3. Windows 8 enhancement

	
    1. 3D video (Stereo 3D)

	
      1. _S3D still not work on CTP version_




	
    2. Audio/Video effect and extensibility

	
    3. Zoom/Mirror

	
    4. Audio output selection

	
    5. Background audio

	
    6. DRM

	
    7. Streaming media to TV and Audio systems




	
  4. System-wide support format

	
    1. File name:

	
      1. MP4/ASF




	
    2. Video:

	
      1. H264/VC1




	
    3. Audio:

	
      1. AAC/MP3/WMA







	
  5. Simple HTML5 video player all related feature: Seeking, bookmark, play/pause, channel switching, capture feature already support on build-in API.

	
  6. Media Focus:

	
    1. When app own media focus video could be see and heard

	
    2. When suspends it will remove from active list, and app will mute

	
    3. When select second media, it will leave original focus to new one.




	
  7. Media format extensibility

	
    1. Custmize your app specific support format

	
    2. Extension are  packaged and local on your app **(only for your app, no share)**

	
    3. Extension could be naive (C++/COM) Media Fundation component.




	
  8. Extending your custom format

	
    1. Register DLL with in App manifest on MF(Media Fundation)

	
      1. Such audio/video source filter or demux




	
    2. In your app, register custom  streaming/data format with ExtensibilityManager

	
      1. Register your custom video/audio encoder/decoder




	
    3. Custom effect could be insert effect pipeline





