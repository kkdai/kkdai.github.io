---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-02-15 07:46:09+00:00
slug: '%e5%a6%82%e4%bd%95%e6%8a%8a%e5%af%ab%e5%a5%bd%e7%9a%84-windows-live-writer-plugin%e5%8c%85%e8%a3%9d%e6%88%90%e5%ae%89%e8%a3%9d%e6%aa%94-how-to-package-your-windows-live-writer-plugin'
title: 如何把寫好的 Windows Live Writer plugin包裝成安裝檔 (How to package your Windows Live writer
  plugin?)
wordpress_id: 1138
categories:
- Windows Live Writer的相關開發
- 學習文件
---

(copy from my another site)

 

由於要修改去年寫給自己用的小工具 ([WLWPlurk](http://wlwplurk.codeplex.com/))，看到其他在Codeplex上面的人都是用WIX(Windows Installer XML)來做包裝plugin的動作。

 

所以也去好好的搜尋一下有沒有相關可以用的東西。 由於我自己也不是專業在做Installer的人~ 裡面的一些設定也真的把我搞糊塗，才了解前因後果。還好找到了相關的文件[WiX Script for installing Live Writer Plugins](http://blogs.msdn.com/b/jmstall/archive/2007/10/27/wix-script-for-installing-live-writer-plugins.aspx)，雖然他上面的sample是針對 WIX 2.0，不過我也把它修改過後分享給大家。

 

希望有需要的人就拿去用吧。

 

  1. Install WIX 3.6 from [http://wix.sourceforge.net/downloadv3.html](http://wix.sourceforge.net/downloadv3.html)
   
  2. Setup your path setting to your install path. 
   
  3. Inser follow code (it should be XXX.WXS)        
(refer from [http://wlwextensionlearning.blogspot.com/2012/02/windows-live-writer-plugin-how-to.html](http://wlwextensionlearning.blogspot.com/2012/02/windows-live-writer-plugin-how-to.html)) 
   
  4. You might need repace follow string as follows:             
    1. {$Plugin Name$} : your plugin name 
       
    2. {$InstallGUID$}: A GUID for your installation program 
       
    3. {$Manufacturer$}: Manufacturer name 
       
    4. {$PluginGUID$}: A GUID for your plugin 
       
    5. {$Description$}: A description for your plugin 
       
    6. {$FILE_ID$}: ID for your binary which need install. (could be multiple file) 
       
    7. {$FILE_NAME$}: Name for your binary which need install. (could be multiple file) 
       
    8. {$FILE_Address$}: File address for your binary which need install. (could be multiple file) 
       
   
  5. Compiler your WIX source code             
    1. candle XXX.wxs 
       
    2. light -ext WixUIExtension XXX.wixobj 
       
