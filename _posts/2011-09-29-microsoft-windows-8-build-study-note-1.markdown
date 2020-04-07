---
layout: post
layout: post
author: kkdai
comments: true
date: 2011-09-29 18:50:27+00:00
slug: microsoft-windows-8-build-study-note-1
title: Microsoft Windows 8 build study NOTE (1)
wordpress_id: 1131
categories:
- 學習文件
- 下一代視窗系統心得
---

 ![](http://files.channel9.msdn.com/thumbnail/ace11650-7aa0-4a31-8689-0890e91146e9.png)

**BPS-1006 Tools for building Metro style apps**

Refer video: [http://channel9.msdn.com/Events/BUILD/BUILD2011/BPS-1006](http://channel9.msdn.com/Events/BUILD/BUILD2011/BPS-1006)

**Note:**

It is introduction of VC2011, here is some major demo topic as follows:



	
  * Auto spell which is powerful with Visual Assist

	
    * Auto spell, finding related function and method. It is easy to go.




	
  * JS debugging:

	
    * It is cool to have debugger for JS which also support all debugging window (variable, watch…)




	
  * Parallel stack debugging windows:

	
    * It has visualization debugging tab for parallel which could show currently threading and each thread dependency.




	
  * RT XAML editor:

	
    * During debugging you also could editing XMAL code by select on UI or select on code segment to show UI change on fly.




	
  * Simulator debugger:

	
    * I think it is very cool stuff for me such we don’t have multiple touch panel on hand but we need implement some related to “Share”, “Search” which need finger slip.

	
    * It also provide “finger”, “rotate”, “zoom” and it could simulate to different resolution for Metro UI debugging.




	
  * Asynchronous programming (await, async)

	
    * Refer here for more detail:

	
      * [Something about C# 5.0](http://tech.hexun.com.tw/2011-05-24/129908043.html)

	
      * [Asynchronous Programming in C# 5.0 part two: Whence await?](http://blogs.msdn.com/b/ericlippert/archive/2010/10/29/asynchronous-programming-in-c-5-0-part-two-whence-await.aspx)




	
    * Concept:

	
      * Will folk another thread after “await” and response on original message loop to ensure response quickly.

	
      * It look like to cut original message thread,one back to main and another keep doing async task.

	
      * It help us to using linear thinking to do asynchronous programming




	
    * Sample: (from [here](http://blogs.msdn.com/b/ericlippert/archive/2010/10/29/asynchronous-programming-in-c-5-0-part-two-whence-await.aspx))
async void ArchiveDocuments(List<Url> urls)
{
Task archive = null;
for(int i = 0; i < urls.Count; ++i)
{
var document = await FetchAsync(urls[i]);
if (archive != null)
await archive;
archive = ArchiveAsync(document);
}
}






**BPS-1005 Platform for Metro style apps**

Refer video: [http://channel9.msdn.com/Events/BUILD/BUILD2011/BPS-1005](http://channel9.msdn.com/Events/BUILD/BUILD2011/BPS-1005)

**Note:**



	
  * What is major different between Metro UI application with desktop application?

	
    * No overlay window architect (all full screen)

	
    * No message box related API.




	
  * Device access all using asynchronous to ensure all application could response on time.

	
  * About “Broker” in WinRT concern as follows:

	
    * Impact system integrity.

	
    * Impact user data

	
    * Impact User private




	
  * HTML5/CSS/JS great for Metro Apps

	
    * MSFT try to enlarge their engineer base and try to draw Web engineer’s attention to join Metro Apps development.




	
  * Package your application:

	
    * Application could be easily to pack and upload to MSFT with one click on VS11.

	
    * All package need digital signature before upload to App Store





**TOOL-789C Bringing existing C++ code into Metro style apps**

Refer video: [http://channel9.msdn.com/Events/BUILD/BUILD2011/TOOL-789C](http://channel9.msdn.com/Events/BUILD/BUILD2011/TOOL-789C)

Note:



	
  * About Metro UI app:

	
    * No registry, GDI, MessageBox coding.




	
  * Here is work/not work desktop app

	
    * Work:

	
      * Standard C++

	
      * Parallel pattern

	
      * Win32 (WinRT and Win32 subset)

	
      * ATL (ATL subset)




	
    * Not work:

	
      * MFC







	
  * About WinRT

	
    * Refer: [Windows Runtime(WinRT) 揭秘](http://www.cnblogs.com/shanyou/archive/2011/09/17/2179699.html)

	
    * Any old DLL component could be easy to pack to WinRT  component if no depends limited list as above.




	
  * Q&A

	
    * Two Metro UI don’t have any way to communicate for now, currently it still isolate.

	
    * For disable API, it might change depends engineer feedback to MSFT.

	
    * All Metro UI need component could be pack to WinRT component and package today to delivery to end-user via store.

	
    * Shared component is not work on Metro UI, all component must be side-by-side. Because you could not use CoCreateInstance by CLSID, so there is unique CLSID on Metro world.

	
    * Whole app on Metro UI are isolate due to security and system integrity.





