---
layout: post
layout: post
author: kkdai
comments: true
date: 2010-09-06 10:58:00+00:00
slug: '%e7%ac%ac%e4%b8%80%e5%80%8bwindows-live-writer-plugin-hello-world-sample-code'
title: 第一個Windows Live Writer plugin "Hello World" (sample code)
wordpress_id: 1139
categories:
- Windows Live Writer的相關開發
---

這裡敘述的Hello Wolrd主要是利用Ben Hall的一篇文章[Windows Live Writer Plugin - Hello World!](http://blog.benhall.me.uk/2007/09/windows-live-writer-plugin-hello-world.html)。 (Related code and article refer from [this](http://blog.benhall.me.uk/2007/09/windows-live-writer-plugin-hello-world.html))

 

**必要工具:**

 

  
  * Visual Studio (這裡用的是2005) 
   
  * 安裝過的Window Live Writer (這裡裝的是最新版B14.0.8089) 
 

**詳細流程:**

 

  
  * 開啟Visual Studio(2005)，選取C# Project的Class Library。        
     

![WLW_01.jpg](http://farm5.static.flickr.com/4085/4963442386_465b803c9b.jpg)

  
   
  * 講兩個需要用的reference 加入參考，首先打開"References” 按下右鍵。"Add References”        
     

![WLW_02.jpg](http://farm5.static.flickr.com/4104/4962852889_daa084b5de.jpg)

  
   
  * 加入一個"System.Windows.Forms"在COM裡面，此外再加入一個額外Windows Live Writer API DLL。選取”Browse”然後點選C:Program FilesWindows LiveWriterWindowsLive.Writer.Api.dll。        
     

![WLW_03.jpg](http://farm5.static.flickr.com/4129/4963469610_748f67ff2d.jpg)

  
   
  * 加入以下的source code. (This source modify from [Ben Hall’s article](http://blog.benhall.me.uk/2007/09/windows-live-writer-plugin-hello-world.html))         
     

      
    
    using System.Windows.Forms;<br></br>using WindowsLive.Writer.Api;<br></br><br></br>namespace LiveWriterHelloWorld<br></br>{<br></br>    [WriterPluginAttribute<br></br>      ("2f437bf1-fe57-41c8-931a-d20066ea174e", "Hello World!",<br></br>        PublisherUrl = "http://wlwextensionlearning.blogspot.com/",<br></br>        Description = "Insert Hello World! into the blog post")]<br></br>    [InsertableContentSourceAttribute("Hello World!", SidebarText = "Hello World!")]<br></br>    public class HelloWorldPlugin : ContentSource<br></br>    {<br></br>        public override DialogResult CreateContent(IWin32Window dialogOwner, ref string content)<br></br>        {<br></br>            content = "<b>Hello World!</b>";<br></br><br></br>            return DialogResult.OK;<br></br>        }<br></br>    }<br></br>}<br></br>






      



  
  * 複製DLL到C:Program FilesWindows LiveWriterPlugins 然後重新啟動Windows Live Writer應該就會看到這個新的plugin.
      


      


    

![WLW_04.jpg](http://farm5.static.flickr.com/4083/4962935295_be1634911d.jpg)



      



  
  * 直接按下去就會跑出 “Hello World” 



  




**參考文章:**



  






  






  






  






  





    


  
  * [Windows Live Writer Plugin - Hello World!](http://blog.benhall.me.uk/2007/09/windows-live-writer-plugin-hello-world.html) ([Blog.BenHall.me.uk](http://blog.benhall.me.uk/))

      



  
  * [Windows Live Writer SDK](http://msdn.microsoft.com/en-us/library/aa738906.aspx)


