---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-21 09:08:55+00:00
slug: windowsstorec-%e9%97%9c%e6%96%bcmemory-leaks-%e7%9a%84debugging-%e7%ad%86%e8%a8%98
title: '[WindowsStore][C#] 關於Memory Leaks 的debugging 筆記'
wordpress_id: 1356
categories:
- C#相關學習
- NET程式設計
- 下一代視窗系統心得
---

一直以來，Windows Store App由於有GC (Garbage Collection)的關係，所以之前寫的App也都不容易有memory leak的產生．  
最近剛好遇到這樣的問題，也好好的把一些相關的工具都學習了一下．再次筆記一下




**Debugging Memory Leaks之前，你必須了解**






  * 所謂的memory leak在使用者能夠看到的永遠是記憶體被用完crash(或是app 收起來)．但是就像是水管漏水一樣， 積水的地方絕對不是漏水的地方．


  * 所以面對memory leak 的問題，第一件事情就是不斷地簡化復現(reproduce)的步驟．


  * 由於有GC，如果你重複三到五次的動作發現有記憶體長大的情形，絕對不要貿然認為你找到關鍵的步驟．必須要超過10~15次讓系統把記憶體收回去．



    * 通常如果是Frame 的記憶體回收，可能5~10次會發生，WinRT 的部分可能會更久


    * 也可以跑一些其他大記憶體的App讓GC強迫產生


    * 或是放一段時間，讓GC動作．


    * 試著可以加上GC.Collect()的程式碼讓系統快一點回收，不過通常而言產生的效果相當有限．



  * 找到了memory leak的部分的時候，千萬不要選擇使用 Dispose或是 Null 某些component，因為這樣都不是正確的方式．必須要回過頭來去尋找為何該記憶體無法被正常的釋放．




**關於尋找Memory Leak的Tools**




這邊要註解一下，大部份的C#或是.NET的memory dump tool 都只能先把關於 managed code 的部分可以先列印出來  
如果memory leak 出現在WINRT或是自己寫的unmanaged code這樣就比較難去尋找了． 









  * [NP .NET Profiler tool](http://blogs.msdn.com/b/webapps/archive/2012/11/23/troubleshooting-memory-leaks-in-windows-store-apps.aspx)



    * 這個工具還不錯，只要先指定好之後．做好一次的動作就用snapshot 抓下現在的記憶體狀況，最後再把狀況dump出來就可以了


    * 心得：



      * 比起等等會提到的PerfView算是比較好用，當然東西會少一點．不過使用上相當直覺，相當好用．




  * [PerfView](http://www.microsoft.com/en-us/download/details.aspx?id=28567) ([說明網頁](http://msdn.microsoft.com/en-us/magazine/jj721593.aspx))



    * 這是微軟的工具，一樣只能提managed code的memory dump但是資訊更多還可以trace到Windows System Symbol．


    * 使用方法也相當簡單，先dump一次後．開始執行會產生memory leak 的動作然後執行第二次dump． 打開原始跟第二次的report就可以diff然後觀看結果．


    * 心得：



      * 比較建議使用這個畢竟是微軟提供的工具，資訊也比較多．不過在使用的時候如果有用debugger attach 會更慢．






**如何Debugging Memory Leaks**









  * 尋找正確的復現步驟，任何bug都是如此但是對於Windows Store(Mobile) system 而言，需要注意以下狀況



    * 如果發現做一次的步驟，記憶體沒有任何的增長，代表你的方式無法復現．反之則不然


    * 做出一次步驟發現記憶體的增長，不要以為找到復現的步驟．恐怕只是系統還未回收記憶體，建議要做到記憶體爆掉App收掉．



  * 這裡先列出一些在managed code裡面容易發生memory leak的部分:



    * UIElement AddHandler/RemoveHandler



      * 根據[許多的資料](http://stackoverflow.com/questions/151303/addhandler-removehandler-not-disposing-correctly)(還有[這裡](http://bytes.com/topic/visual-basic-net/answers/441527-can-addhandler-cause-memory-leaks))上來說GC應該會處理這樣的Handler，但是透過我實地拿PerfView的結果卻是會產生memory leak． 還是得注意一下．



    * Delegate的處理  += 記得要 -= 回去



      * 這個是一定會出事情的，千萬要注意你的 OnNavigationTo/OnNavigationFrom 有沒有成對的 += 與 -= 



    * 利用PerfvIew 查詢reference count是否有任何的異常產生(也就是查詢到爆量或是超過自己想像的ref cnt)



  * 利用PerfView 來觀察的部分，這邊有一些小技巧可以去觀察，驗證的方法:



    * [觀察]使用Diff方法，一開始先記錄下來後，做完疑似memory leak 的步驟之後，再記錄下來．


    * [驗證]可以把修改前的跟修改後的比對，不過這裡建議用修改前去diff 修改後～雖然數字出來會是正數但是這樣比較容易分析...











**參考文章:**






  * [http://blogs.msdn.com/b/webapps/archive/2012/11/23/troubleshooting-memory-leaks-in-windows-store-apps.aspx](http://blogs.msdn.com/b/webapps/archive/2012/11/23/troubleshooting-memory-leaks-in-windows-store-apps.aspx)


  * [http://msdn.microsoft.com/en-us/magazine/jj721593.aspx](http://msdn.microsoft.com/en-us/magazine/jj721593.aspx)


  * [http://msdn.microsoft.com/zh-tw/magazine/jj651575.aspx#MtViewDropDownText](http://msdn.microsoft.com/zh-tw/magazine/jj651575.aspx#MtViewDropDownText)


  * [http://stackoverflow.com/questions/13730496/how-to-debug-memory-leaks-in-windows-store-apps](http://stackoverflow.com/questions/13730496/how-to-debug-memory-leaks-in-windows-store-apps)


  * [http://channel9.msdn.com/Series/PerfView-Tutorial/Tutorial-10-Investigating-NET-Heap-Memory-Leaks-Part1-Collecting-the-data](http://channel9.msdn.com/Series/PerfView-Tutorial/Tutorial-10-Investigating-NET-Heap-Memory-Leaks-Part1-Collecting-the-data)


  * [http://channel9.msdn.com/Series/PerfView-Tutorial/Tutorial-11-Investigating-NET-Heap-Memory-Leaks-Part2-Analyzing-the-data](http://channel9.msdn.com/Series/PerfView-Tutorial/Tutorial-11-Investigating-NET-Heap-Memory-Leaks-Part2-Analyzing-the-data)


