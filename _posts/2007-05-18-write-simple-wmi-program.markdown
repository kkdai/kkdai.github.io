---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-05-18 04:55:02+00:00
slug: write-simple-wmi-program
title: Write simple WMI program
wordpress_id: 645
categories:
- NET程式設計
---

![](http://www.msservermag.com.tw/technicwords/021128/01.gif)  
(Diagram: **淺談CIM與WMI-**[名詞介紹](http://www.msservermag.com.tw/technicwords/021128.aspx))

[WMI](http://www.microsoft.com/whdc/system/pnppwr/wmi/default.mspx) (Windows Management Instrumentation) provide a kind of easy way to let you query data from your system (Ex: OS version, share folder name ... etc). 

It is easy to understand, using [WMI](http://www.microsoft.com/whdc/system/pnppwr/wmi/default.mspx) to query information is the same with query data from database. You can just using "SELECT * from Win32_Process" to find out all exist process in your computer.

There are some examples which describe how to create it using C# or C++ code.

  1. [Example: Creating a WMI Application](http://msdn2.microsoft.com/en-us/library/aa390418.aspx)(C++)

  2. [WMI Sample  (using C#)](http://msdn2.microsoft.com/en-us/library/ms173052(vs.80).aspx)

There are serveral extension reading..

  * [**Brightness** Control in WDDM](http://download.microsoft.com/download/a/f/7/af7777e5-7dcd-4800-8a0a-b18336565f5b/Brightness.doc)

  * [ACPI / Power Management - Architecture and Driver Support](http://www.microsoft.com/whdc/system/pnppwr/powermgmt/default.mspx)

Here is sample code of C#

<blockquote>using System.Management;  
  
//This is equivalent to "SELECT * FROM Win32_Service"  
SelectQuery sysSQL = new SelectQuery("Win32_bios");
> 
> MnagementObjectSearcher sysRet =   
   new ManagementObjectSearcher(sysSQL);
> 
> // Display each entry for Win32_bios  
foreach (ManagementObject sysInfo in sysRet.Get())  
{  
  this.textBox1.Text = "Bios version: " + sysInfo["version"].ToString();   
}
> 
> </blockquote>
