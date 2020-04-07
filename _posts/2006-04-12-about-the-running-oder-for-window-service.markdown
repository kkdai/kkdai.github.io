---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-04-12 09:38:38+00:00
slug: about-the-running-oder-for-window-service
title: About the running oder for Window Service
wordpress_id: 380
categories:
- VC++程式設計
---

![WindowsService (WinCE).JPG](http://www.evanlin.com/blog/archives/20060217/WindowsService%20(WinCE).JPG) 

We usually doesn't care about the startup odering of Window Services. But there is a situation about this.

**You usually don't care what you don't understand.**

Total Service: _ServiceA and ServiceB._

In ServiceA we will retrieval some information via ServiceB. If ServiceB doesn't start before ServiceA this future will failed. Because all Window Service are controled by SCM ("Service Control Manager).

**How do we make sure the startup odering of Window Services?**

Actually, SCM dispath all service ramdonly, such that you can not promise the services startup odering. But~~  (mm Here comes a hreo~~ ).

We can set the dependency of every service.

[![ServiceDependency (Small).JPG](http://www.evanlin.com/blog/archives/20060412/ServiceDependency%20(Small)-thumb.JPG)](http://www.evanlin.com/blog/archives/20060412/ServiceDependency%20(Small).JPG)   


You can try to open SCM and open a service propertied dialog to see the same information as bellow.

In the table of "Dependency", you will see all depdency services of this service. It will make sure when this service starup all dependency service should already startup. You should set when you create such service. 

<blockquote>::CreateService(hSCM,     // SCM pointer  
  m_szServiceName,   // service identify name  
  m_szServiceName,   // Display service name to display   
   SERVICE_ALL_ACCESS,  // desired access  
  SERVICE_WIN32_OWN_PROCESS, // service type   
  SERVICE_AUTO_START,  // start type   
  SERVICE_ERROR_NORMAL,  // error control type   
  szFilePath,   // service's binary   
  NULL,    // no load ordering group   
  NULL,    // no tag identifier   
  _T("RPCSS"),   // Dependencies on RPC Call Service.  
  NULL,    // LocalSystem account   
  NULL);    // no password 
> 
> </blockquote>

Please reference [MSDN for more detail](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/createservice.asp).
