---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-08-16 07:04:03+00:00
slug: how-to-login-as-administrator-in-windows-xp-and-vista
title: How to login as Administrator in Windows XP and Vista?
wordpress_id: 466
categories:
- 下一代視窗系統心得
---

[![](http://images.google.com.tw/images?q=tbn:eJhTLW4R6S6viM:www.kiruthik.com/content/binary/Windows_Vista_logo.jpg)](http://images.google.com.tw/imgres?imgurl=http://www.kiruthik.com/content/binary/Windows_Vista_logo.jpg&imgrefurl=http://www.kiruthik.com/CategoryView,category,Technology.aspx&h=269&w=367&sz=16&hl=zh-TW&start=1&tbnid=eJhTLW4R6S6viM:&tbnh=89&tbnw=122&prev=/images%3Fq%3Dvista%2Blogo%26svnum%3D10%26hl%3Dzh-TW%26lr%3D)

### In Vista:

First you must to enable "Administrator" account in your OS. 

My Computer (right click) --> [Manage] --> [Users and Groups] --> Users --> Administrator --> Properties. 

Then do the same thing as WinXP do.

### In WinXP:

### Method 1: Using TweakUI Power Toy for Windows XP

Download TweakUI from here:

[v2.00 for Windows XP](http://download.microsoft.com/download/whistler/Install/2/WXP/EN-US/TweakUiPowertoySetup.exe)  |  [v2.10 for XP SP1 and above](http://download.microsoft.com/download/f/c/a/fca6767b-9ed9-45a6-b352-839afb2a2679/TweakUiPowertoySetup.exe)

Open TweakUI and click "Logon" option in the left pane. Put a checkmark against the option "Show Administrator on Welcome Screen". Click OK to close TweakUI. Logoff and see if Welcome Screen lists Administrator login. Changes are immediate and you can use the Winkey + L to switch back to Welcome Screen to see Administrator account is listed.

**

### Method 2 - Manual registry edit

  * Click Start, Run and type **Regedit.exe**

  * Navigate to the following key:  
  
HKEY_LOCAL_MACHINE  SOFTWARE  Microsoft  Windows NT  CurrentVersion  Winlogon  SpecialAccounts  UserList  
 

  * Use the File, Export option to [backup the ](http://windowsxp.mvps.org/registry.htm)[key](http://windowsxp.mvps.org/registry.htm).

  * Right-click in the right pane and select New DWORD Value.

  * Type-in Administrator as the value. 

  * Double-click Administrator, and assign a value of **1**

  * Close Regedit

**

**Reference:**

[http://windowsxp.mvps.org/admins.htm](http://windowsxp.mvps.org/admins.htm)
