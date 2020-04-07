---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-03-07 09:39:11+00:00
slug: how-to-communicate-between-metro-ui-and-desktop-ui-on-win8
title: How to communicate between Metro UI and Desktop UI on Win8
wordpress_id: 1152
categories:
- VC++程式設計
- 下一代視窗系統心得
---

It just a summarize to explain MSFT sample: [Association launching sample](http://go.microsoft.com/fwlink/?LinkId=231484).



	
  * Metro UI to call Desktop UI

	
    * File extension call:

	
      * Add default open on registry if you want create new one.

	
        * [HKEY_CLASSES_ROOT.XXX]




	
      * Using Windows.System.Launcher.launchFileAsync




	
    * Protocol call: (such as “http://”, “mailto://”)

	
      * Add default protocol on registry if you want create new one.

	
        * [HKEY_CLASSES_ROOT%PROTOCOL%] [HKEY_CLASSES_ROOT\%PROTOCOL%DefaultIcon]
@="C:\Program Files\XXX.exe,0"[HKEY_CLASSES_ROOT\%PROTOCOL%shell]
@="play"

[HKEY_CLASSES_ROOT\%PROTOCOL%shellopen]
@=""

[HKEY_CLASSES_ROOT\%PROTOCOL%shellopencommand]
@=""C:\Program Files\XXX.exe" %1"

[HKEY_CLASSES_ROOT%PROTOCOL%shellplaycommand]
@=""C:\Program Files\XXX.exe" %1"




	
      * Using Windows.System.Launcher.launchUriAsync








P.S.: Since it is Async, please note you might need handle it well to make it work. :)

	
  * 


Metro UI to call Desktop UI




	
    * 


File extension call




	
      * 


Add on “package.appxmanifest” “Declarations” to add “Protocol”.




	
      * 


Handle callback feedback and launch this app.




	
      * 


Double click file on desktop UI.







	
    * 


Protocol call: (such as “http://”, “mailto://”)




	
      * 


Add on “package.appxmanifest” “Declarations” to add “File Type Associations”. (ex: “sampleApp://”)




	
      * 


Handle callback feedback and launch this app.




	
      * 


Type “sampleApp://” on file browser or IE.




	
      * 


You could using code “ShellExecute(NULL, _T("open"), _T("sampleApp://"), NULL, NULL, SW_SHOWNORMAL);”













**How we use it?**






	
  * 


To transfer control between Metro/Desktop.




	
  * 


To Write a launcher on Metro UI for your desktop application.




	
  * 


Express app on Metro UI and Pro app on desktop.







**Refer:**






	
  * Auto-launching with file and protocol associations: [http://msdn.microsoft.com/en-us/library/windows/apps/hh452691.aspx](http://msdn.microsoft.com/en-us/library/windows/apps/hh452691.aspx)


