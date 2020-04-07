---
layout: post
layout: post
author: kkdai
comments: true
date: 2011-12-01 05:56:23+00:00
slug: microsoft-windows-8-build-study-note-4
title: MICROSOFT WINDOWS 8 BUILD STUDY NOTE (4)
wordpress_id: 1137
categories:
- 學習文件
- 下一代視窗系統心得
---

![](http://files.channel9.msdn.com/thumbnail/ace11650-7aa0-4a31-8689-0890e91146e9.png)

HW-59T - Improving performance with the Windows Performance Toolkit

refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/HW-59T](http://channel9.msdn.com/Events/BUILD/BUILD2011/HW-59T)

Note:



	
  * Motivation for performance analysis:

	
    * Performance related to multiple layered impact such as : hardware, OS, other application and process impact.




	
  * Stage of performance improvement:

	
    * Preception –> Measurement –> Analysis.

	
    * It will be back and forth after analysis and re-preception.




	
  * Tool list:

	
    * Trace Capture: Windows Performance Recorder

	
    * Tacce Analysis: Windows Performance Analizer




	
  * Things to look out:

	
    * Don’t assume you know what’s wrong.

	
    * Don’t enable too much instrumentation

	
    * Don’t trace longer than you need.





PLAT-203T Async everywhere: creating responsive APIs & apps

refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-203T](http://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-203T)

Note:



	
  * Async only support on some API:

	
    * XAML, Tile on UI

	
    * GPS and portable debice on Device

	
    * All network and communicate

	
    * Applifetime, Authentication, Crytography on Fundamental




	
  * Await

	
    * Await is a simple way to against async API.

	
    * Await will make the async highest priority and let the compiler know here need to wait the result come back. Code will run as sequencial.




	
  * Error handling


<blockquote>try {

FileOpenPicker p = new FileOpenPicker();

p.FileTypeFilter.Add(".jpg");ry

var operation = p.PickSingleFileAsync();

operation.Completed = (IAsyncOperation<StorageFile> f) =>

{

MyButton.Content = f.GetResults().FileName;

};

operation.Start();

catch(...) {}</blockquote>



	
    * try catch will only cover RED one, it need refine code to cover whole scope




<blockquote>try {

FileOpenPicker p = new FileOpenPicker();

p.FileTypeFilter.Add(".jpg");

MyButton.Content =

(await p.PickSingleFileAsync()).FileName;

} catch(Exception e) {}</blockquote>



	
    * It will separate into “RED” and “Green” one but still cover on try catch.




	
  * 5 top tips from presenter.

	
    * Don’t worry about the COM apartment.

	
    * Remember to start your operation.

	
    * File picker cancel != Async Cancel

	
      * It will complete but return NULL.

	
      * “use exception for exceptional things”




	
    * Don’t worry about Dispatcher.Invoke…

	
    * Don’t worry about the concurrency





Refer: [http://blogs.msdn.com/b/ericlippert/archive/2010/10/29/asynchronous-programming-in-c-5-0-part-two-whence-await.aspx](http://blogs.msdn.com/b/ericlippert/archive/2010/10/29/asynchronous-programming-in-c-5-0-part-two-whence-await.aspx)
