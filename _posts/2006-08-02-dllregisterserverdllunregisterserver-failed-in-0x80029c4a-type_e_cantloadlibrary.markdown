---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-08-02 04:08:57+00:00
slug: dllregisterserverdllunregisterserver-failed-in-0x80029c4a-type_e_cantloadlibrary
title: DllRegisterServer/DllUnregisterServer failed in 0×80029C4A (TYPE_E_CANTLOADLIBRARY)
wordpress_id: 456
categories:
- VC++程式設計
---

![](http://www.gotdotnet.com/team/ide/images/image002.jpg)

**SYMPTOMS  
**When you try to register or unregister a COM server, it will pop failed in 0x80029C4A (TYPE_E_CANTLOADLIBRARY).

**CAUSE  
**This is cuase by no TLB or missing TLB of this COM DLL.   


**RESOLUTION  
**If your COM DLL is builded using /notlb. remember use  
     CComModule::RgisterServer(FALSE);  
     CComModule::DllUnregisterServer(FALSE);  


otherwise, please make sure your TLB is well for register.

**REFERENCE**

[CComModule::RegisterServer](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dv_wceatl4/html/ealrfCComModulecolcolRegisterServer.asp); [CComModule::UnregisterServer](http://msdn2.microsoft.com/zh-CN/library/9s1yde5e.aspx)
