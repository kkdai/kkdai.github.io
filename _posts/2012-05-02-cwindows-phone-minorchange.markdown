---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-05-02 03:07:04+00:00
slug: cwindows-phone-%e9%97%9c%e6%96%bc%e9%a0%81%e9%9d%a2%e8%99%95%e7%90%86%e7%9a%84%e7%9b%b8%e9%97%9c%e5%b0%8f%e7%b4%b0%e7%af%80%e6%94%b9%e8%ae%8a%e5%88%9d%e5%a7%8b%e9%a0%81%e9%9d%a2%e8%88%87
title: '[C#][WINDOWS PHONE] 關於頁面處理的相關小細節(改變初始頁面與頁面間傳遞參數)'
wordpress_id: 1162
categories:
- NET程式設計
---

這幾天開始把原來的RSS Reader APP程式多加頁面後，發現了一些小問題。網路上或許還算是好找。

 

但是我稍微整理一下，其實Pou’s blog兩篇文章有相當清楚的解釋

 

  
  * [Windows Phone 7 - 動態轉換初始的Default Page](http://www.dotblogs.com.tw/pou/archive/2011/04/30/23929.aspx)
   
  * [Windows Phone 7 – Navigation Framework原理概論](http://www.dotblogs.com.tw/pou/archive/2011/01/23/20967.aspx)
 

在此只把常遇到的兩個問題整理一下:

 

  
  1. 如何改變初始頁面(How to change default page on Windows Phone)        
修改文件”WMAppManifest.xml”以下程式碼:         
     
    
    <Tasks>
      <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
    </Tasks>


  


  
  2. 如何在頁面中傳遞參數(How to pass parameter over pages on Windows Phone)
      
在頁面間傳遞參數可以把它當成是網頁的GET模式(就是?Param_1=value_1&Param_2=value_2)

      
在原來的頁面可以用以下的指令傳遞

      


    
    
    string mylogin = "/Page2.xaml";
    mylogin += "?Param1=" + "VALUE1";
    mylogin += "&Param2=" + "VALUE2";
    
    if (!String.IsNullOrWhiteSpace(mylogin))
    {
    	this.NavigationService.Navigate(new Uri(mylogin, UriKind.Relative));
    }



      
再接收的Page2就必須要有以下的方式去接受參數

      


    
    
    protected override void OnNavigatedTo(System.Windows.Navigation.NavigationEventArgs e)
    {
                //Get parameter
                string myParam1 = NavigationContext.QueryString["Param1"];
                string myParam2 = NavigationContext.QueryString["Param2"];
    }


  





這樣就可以了~~~





參考網頁:






  
  * [http://www.eugenedotnet.com/2011/03/passing-values-to-windows-phone-7-pages-uri-paremeters-or-querystring/](http://www.eugenedotnet.com/2011/03/passing-values-to-windows-phone-7-pages-uri-paremeters-or-querystring/)


  
  * [http://stackoverflow.com/questions/3892271/how-do-i-change-the-startup-page-on-a-wp7-silverlight-app](http://stackoverflow.com/questions/3892271/how-do-i-change-the-startup-page-on-a-wp7-silverlight-app)


  
  * [http://www.dotblogs.com.tw/pou/archive/2011/06/05/27148.aspx](http://www.dotblogs.com.tw/pou/archive/2011/06/05/27148.aspx)


  
  * [http://www.dotblogs.com.tw/pou/archive/2011/04/30/23929.aspx](http://www.dotblogs.com.tw/pou/archive/2011/04/30/23929.aspx)


  
  * [http://www.dotblogs.com.tw/pou/archive/2011/01/23/20967.aspx](http://www.dotblogs.com.tw/pou/archive/2011/01/23/20967.aspx)


