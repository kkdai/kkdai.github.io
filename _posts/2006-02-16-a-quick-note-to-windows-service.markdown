---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-02-16 10:14:43+00:00
slug: a-quick-note-to-windows-service
title: A Quick note to “Windows Service”
wordpress_id: 338
categories:
- VC++程式設計
---

![WindowsService (WinCE).JPG](http://www.evanlin.com/blog/archives/20060217/WindowsService%20(WinCE).JPG)

A simple note from "Write Pure Windows Service" and "How to debug Windows Service".  
  


**Write "Windows Service"**

Windows Service is kind of application but it will start when system startup. Windows service will triggerd by "SMC"(System Managed Controller). There is some important note from my writing windows service experience.

  1. **Register your service:  
**What ever your application is big or small. You must register them to SMC before you use it. Please reference [_::OpenSCManager()_ ](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/openscmanager.asp)and [_::CreateService()._](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/createservice.asp)   
For more detail please reference[ Writing a Service Program's main Function](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/writing_a_service_program_s_main_function.asp)  

  2. **Start a "Windows Service".**  
You can not startup a windows service via any debugger, you need to startup it on SMC. The SMC will use [_::StartServiceCtrlDispatcher_ ](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/startservicectrldispatcher.asp)to start service and wait for the call back signal "**SERVICE_RUNNING**". If you can not startup windows service normally, please let the Service_Status call back and tried to figurate out the root cause. For more detail please refer to [Installing a Service](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/writing_a_service_program_s_main_function.asp)  

  3. **Stop a Windows Service**  
When you tried to close a windows service from SMC.The handler will receive a "**SERVICE_CONTROL_STOP**" from system. Your program have to pass "**SERVICE_STOP_PENDING**" and call [_RevokeClassObjects()_ ](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/vclib/html/_atl_CComModule.3a3a.RevokeClassObjects.asp). For more detail please refer to [Stopping a Service](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/writing_a_service_program_s_main_function.asp).

**Debugging "Windows Service"**

There are two "easy" ways to debug your Windows service.

  1. **Attach Windows service by debugger:**  
In VC6 "Build"--> "Start Debug" -->"Attach a process" to point out your service it will use debugger to debug the service.  

  2. **"Debugging Windows Service"**   
use "_[DebugBreak();](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/debug/base/debugbreak.asp)_". it will pop up the Window and allow you to debug into Windows Service(You must install VC6 or higher version).   

  3. For more detail, please refer this address([How to debug Windows services](http://support.microsoft.com/?kbid=824344)).

At finally, [there is a best practice for you](http://softlab.technion.ac.il/project/ProxiProject/Src/ProxiServer/ProxiServer.cpp).

_2006-03-20:(Update)_

Please note:  
1. VC7 or VC8 may have some problem when attach the VC6 debug version. (I think it is the pdb doesn't match).  
2. To attach the thread. you may need to turn on the "Show system process", it will show the Windows Service process.

_2006-04-12:(Update)_

[Download Beeper Service Simple Code](http://www.evanlin.com/blog/archives/20060412/Beeper%20Service.cpp)
