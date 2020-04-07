---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-06-02 03:08:46+00:00
slug: runtime-to-startstop-another-window-sevice
title: Runtime to start/stop another window sevice
wordpress_id: 405
categories:
- VC++程式設計
---

![WindowsService (WinCE).JPG](http://www.evanlin.com/blog/archives/20060217/WindowsService%20(WinCE).JPG) 

Except to [set the dependency of services](http://www.evanlin.com/blog/archives/000472.html) in SCM(Service Control Manager). You have another way to make sure some services can run before your application (serivce).

**Use StartService to start a Window Service**

Please note as follow:

  1. If the service already start, it will return FALSE. 
  2. The function will return if the Windows Service is in running status (maybe needs 30sec atmost) or failed.

For more detail, please [refer MSDN](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/startservice.asp). Refer the sample code.

<blockquote> SC_HANDLE serviceControlManager = OpenSCManager(0, 0, SC_MANAGER_CONNECT);  
 SC_HANDLE service;
> 
>  if (serviceControlManager)  
  {  
  service = OpenService(serviceControlManager,  
         "SERVICE_NAME",   
         SERVICE_START | SERVICE_QUERY_STATUS | DELETE);  
// Make sure you have the privilege of SERVICE_START.
> 
>   if (service)  
   {  
   if (StartService(service, 0, NULL))  
    {  
    // Services already startup successes.  
    }  
   else   
    {  
    DWORD dwLastError = GetLastError();  
    if (dwLastError == ERROR_SERVICE_ALREADY_RUNNING)  
     {  
     //Services already started  
     }  
    else  
     {  
     //Service can not start.  
     hr = E_FAIL;  
     return;  
     }  
    }  
   }  

> 
> </blockquote>

**Use ControlService to stop Windows Services**

You may think it may have a function StopService to stop serivce. Unfortunately, you have use [CotrolService](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/controlservice.asp) to do that. Please refer the sample code for more detail.

<blockquote>SC_HANDLE serviceControlManager = OpenSCManager(0, 0, SC_MANAGER_CONNECT);  
 SC_HANDLE service;
> 
>  if (serviceControlManager)  
  {  
  service = OpenService(serviceControlManager,  
         "SERVICE_NAME",   
         SERVICE_STOP | SERVICE_QUERY_STATUS | DELETE);  
// Make sure you have the privilege of SERVICE_STOP.
> 
>   if (service)  
   {  
   SERVICE_STATUS serviceStatus;  
   QueryServiceStatus( service, &serviceStatus );
> 
>    if (serviceStatus.dwCurrentState != SERVICE_STOPPED)  
    {  
    if (! ControlService(service,    // handle to service   
          SERVICE_CONTROL_STOP,  // control value to send   
          &serviceStatus) )  // address of status info   
      {  
      // ControlService failed   
      }  
    }  
   }  
  } 
> 
> </blockquote>
