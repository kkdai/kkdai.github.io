---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-11-27 08:43:00+00:00
slug: cwindows-store-app-facebook-dashboard-rss-reader
title: '[C#][Windows Store App] Facebook Dashboard RSS Reader'
wordpress_id: 1169
categories:
- NET程式設計
- 學習文件
- 下一代視窗系統心得
---

雖然自己有需求~但是寫在Windows 8主要也是拿來練功~      
順便看看新版VS2012 RTM的版本有那些Windows Store App 的API修改。

 

產品功能簡介:

 

  
  * 登入Facebook將你塗鴉牆上面的資料下載下來。(約最近2X筆) 
   
  * 在Windows Store App上有列出兩個類別 (All, UnRead) 
   
  * 讀過的在Unread 將不會再出現 
   
  * 支援離線閱讀 
 

簡單的記下以下幾個經過:

 

  
  * 安裝 C# Facebook SDK [http://csharpsdk.org/](http://csharpsdk.org/)       
    * 需要裝NuGet是比較特別的~不過2012裝超快 
       
   
  * 如何使用與申請FACEBOOK ID 請看以下介紹文章             
    * [http://www.dotblogs.com.tw/remhom/archive/2012/03/08/70581.aspx](http://www.dotblogs.com.tw/remhom/archive/2012/03/08/70581.aspx)
       
    * [http://www.dotblogs.com.tw/dominic/archive/2012/06/06/72647.aspx](http://www.dotblogs.com.tw/dominic/archive/2012/06/06/72647.aspx)
       
   
  * 利用Split view 為基礎開始寫，使用SampleDataSource 
   
  * “me/home” 就可以直接讀取讀塗鴉牆 (limit 可以限制筆數) 
   
  * 儲存資料使用以下兩種             
    * ApplicationData.Current.RoamingFolder 存取access token 
       
    * ApplicationDataContainer 資料很方便~ 適合存取JSON吐回來的資料。 
       
   
  * 唯一遇到困難是 enum ApplicationDataContainer，不過[解決](http://social.msdn.microsoft.com/Forums/en-US/winappswithcsharp/thread/398640ff-60a8-4ee2-a2f5-a17be7e4bfc2/#c9c05f5a-4736-46fc-8cfd-cc134d72065d)了。 
 

整個時間只有花費五個小時左右~ 也代表C# Windows Store App真的很方便~      
正在研究如何把Windows Store App 弄出來放上codeplex~
