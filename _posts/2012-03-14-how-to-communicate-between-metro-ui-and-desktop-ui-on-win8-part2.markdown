---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-03-14 09:01:54+00:00
slug: how-to-communicate-between-metro-ui-and-desktop-ui-on-win8-part2
title: HOW TO COMMUNICATE BETWEEN METRO UI AND DESKTOP UI ON WIN8 (part2)
wordpress_id: 1153
categories:
- NET程式設計
- VC++程式設計
- 下一代視窗系統心得
---

From last article “HOW TO COMMUNICATE BETWEEN METRO UI AND DESKTOP UI ON WIN8”, it talking about to pass the control between Metro/Desktop application.

However during programing on Metro and Desktop, you might need some communication mechanism for Metro/Desktop application.

I just provide my experience for this kind of question, here is my solution for communicate between Metro and Desktop on Win8 Customer Preview.  (I also feedback on MSFT forum [here](http://social.msdn.microsoft.com/Forums/eu/winappswithnativecode/thread/41601dad-da3f-419e-91a6-e17467802926))



	
  1. Write a application to listen TCP as information center which call "DesktopSvr" (ex: [here](http://www.codeproject.com/Articles/1608/Asynchronous-socket-communication))

	
  2. Metro app try to send message to localhost via streamsocket (P.S. localhost is not work, but 127.0.0.1 work well).

	
  3. "DesktopSvr" will pass desktop app via IPC or anyway you familiar.


Metro application communication guidline:

	
  * Desktop might also pass some data to “DesktopSvr” it will be our information center.

	
  * Metro:

	
    * Since Metro UI will not get any TCP feedback if app under suspends, so communicate between metro app via TCP seems not possible here.

	
    * Metro app only get TCP event when it is remain as alive. (ex: [StreamSocket](http://code.msdn.microsoft.com/StreamSocket-Sample-8c573931).)

	
    * Metro app only take feedback and response when event register. (ex: [SystemTriggerType](http://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.systemtriggertype.aspx)), so Metro app might need take more pooling query information to our “DesktopSvr”.

	
    * Metro app might not to communicate to another Metro app, it is possible for “Metro A” to pass to “DesktopSvr” and “Metro B” also try to retrival information to “Desktop”.




	
  * Desktop:

	
    * Desktop is easy to communicate with other desktop app via original way.

	
    * Desktop might also need pass information to “DesktopSvr” first, and Metro app will try to get it from “DesktopSvr”.





