---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-08-02 12:57:44+00:00
slug: remote-debug-in-vs2005
title: Remote debug in VS2005
wordpress_id: 457
categories:
- NET程式設計
---

![](http://msdn.microsoft.com/vstudio/images/VS2005_logo_product_home.gif)

Reference: 

  * [http://msdn2.microsoft.com/zh-TW/library/bt727f1t.aspx](http://msdn2.microsoft.com/zh-TW/library/bt727f1t.aspx)
  * [http://msdn.microsoft.com/netframework/programming/64bit/remotedebugging/](http://msdn.microsoft.com/netframework/programming/64bit/remotedebugging/)

  1. Follow the reference for necessary configuration on local project and remote machine's network settings.
  2. Install the debug package on the remote machine and launch the debug monitor. Under the installation diretory [_Microsoft_](//\Microsoft) Visual Studio 2005vsRemote Debuggerx86
  3. Startup "Visual Studio 2005 Remote Debugger", and use "No Authentication"(Naive only).  

  4. Copy follow folders from local machine if needed.  
C:WINDOWSWinSxSManifestsx86_Microsoft.VC80.DebugCRT_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_f75eb16c.cat  
C:WINDOWSWinSxSManifestsx86_Microsoft.VC80.DebugCRT_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_f75eb16c.manifest  
C:WINDOWSWinSxSManifestsx86_Microsoft.VC80.DebugMFC_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_c8452471.cat  
C:WINDOWSWinSxSManifestsx86_Microsoft.VC80.DebugMFC_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_c8452471.manifest  
C:WINDOWSWinSxSManifestsx86_Microsoft.VC80.DebugOpenMP_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_66b81908.cat  
C:WINDOWSWinSxSManifestsx86_Microsoft.VC80.DebugOpenMP_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_66b81908.manifest  
C:WINDOWSWinSxSPoliciesx86_policy.8.0.Microsoft.VC80.DebugCRT_1fc8b3b9a1e18e3b_x-ww_09e017b4  
C:WINDOWSWinSxSPoliciesx86_policy.8.0.Microsoft.VC80.DebugMFC_1fc8b3b9a1e18e3b_x-ww_a193936f  
C:WINDOWSWinSxSPoliciesx86_policy.8.0.Microsoft.VC80.DebugOpenMP_1fc8b3b9a1e18e3b_x-ww_6afafa78  
C:WINDOWSWinSxSx86_Microsoft.VC80.DebugCRT_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_f75eb16c  
C:WINDOWSWinSxSx86_Microsoft.VC80.DebugMFC_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_c8452471  
C:WINDOWSWinSxSx86_Microsoft.VC80.DebugOpenMP_1fc8b3b9a1e18e3b_8.0.50727.42_x-ww_66b81908
  5. Copy your debug code in debug machine.
  6.  Change your project setting as follow.

![](http://msdn.microsoft.com/netframework/art/Project%20Debugging%20Properties.GIF)
