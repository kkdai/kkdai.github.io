---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-07-14 13:58:10+00:00
slug: about-window-autoplay-on-vista
title: About Window autoplay on Vista
wordpress_id: 903
categories:
- 下一代視窗系統心得
---

[![autoplay.jpg](http://static.flickr.com/3229/2666293419_eaa9239852.jpg)](http://www.flickr.com/photos/27643002@N00/2666293419/)

 

CD-ROM Does Not Run Automatically When Inserted      
[http://support.microsoft.com/kb/177880/en-us](http://support.microsoft.com/kb/177880/en-us)

 

當插入 CD - ROM 不會自動執行      
[http://support.microsoft.com/kb/q177880/](http://support.microsoft.com/kb/q177880/)

 

Windows Media Player Does Not Play Audio CD-ROMs Automatically      
[http://support.microsoft.com/kb/279614/en-us](http://support.microsoft.com/kb/279614/en-us)

 

Audio CD Auto Play      
[http://forums.techguy.org/multimedia/37294-audio-cd-auto-play.html](http://forums.techguy.org/multimedia/37294-audio-cd-auto-play.html)

 

**CD Autorun        
[http://www.vista-xp.co.uk/forums/faqs-tips-tricks/147-cd-autorun.html](http://www.vista-xp.co.uk/forums/faqs-tips-tricks/147-cd-autorun.html)         
**Edit the registry : look for:       
HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesCdromon "AutoRun":       
"1" for enable       
"0" for disable       
--------------------------------------------------------------------------------       
[From Microsoft Knowledge Base Article - 330135](http://support.microsoft.com/kb/330135)... quote:       
A value of 0xb5 in the following registry key turns off the AutoRun feature for CDs:       
HKEY_CURRENT_USERSOFTWAREMicrosoftWindowsCurrentVersionPoliciesExplorerNoDriveTypeAutoRun       
You must set the hexadecimal value to 91 to enable the AutoRun feature.

 

**If you want enable double click on my computer for audio CD, just try to create file association for "*.cda" for specific appliation.        
想要啟動雙擊光碟片在"我的電腦"上? 就把"檔案關聯" *.cda跟想要建立的應用程式，建立起來。**
