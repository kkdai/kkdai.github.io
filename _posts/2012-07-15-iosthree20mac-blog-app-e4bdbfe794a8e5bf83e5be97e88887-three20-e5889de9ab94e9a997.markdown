---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-07-15 08:44:12+00:00
slug: iosthree20mac-blog-app-%e4%bd%bf%e7%94%a8%e5%bf%83%e5%be%97%e8%88%87-three20-%e5%88%9d%e9%ab%94%e9%a9%97
title: '[iOS][Three20]Mac Blog App 使用心得與 Three20 初體驗'
wordpress_id: 1165
categories:
- iOS的程式開發
---

從上個禮拜MBA入手之後，就開始研究關於Mac OS上面的一些操作。




首先要先弄好的就是撰寫Blog的系統，以下是一些研究心得：




 




Mac上撰寫Blog系統使用心得：  
1. Ecto: 其實很好用，但是有著致命的空行問題，看來不會修。放棄！  
2. Qumana: 對於貼上程式碼支援不好，貼圖也不方便。  
3. Blogo: 連Launch都不行，應該跟我的版本有關，繼續找找。  
4. ScriptFile:不算是App因為是Chrome extension (Filefox也有），支援算差！  
5. MarsEdit: 用到現在最佳的，不可以編輯隱藏blog，但是貼程式碼相當順暢！




參考文章：






	
  * [http://macoolife.com/blog/2/](http://macoolife.com/blog/2/)


	
  * [http://www.pjhome.net/article/Diary/1034.html](http://www.pjhome.net/article/Diary/1034.html)

	
  * [http://www.icyif.com/archives/1699.html](http://www.icyif.com/archives/1699.html)

	
  * [http://atlasweng.blogspot.tw/2010/02/mac.html](http://atlasweng.blogspot.tw/2010/02/mac.html)




 




這些試用過後，應該會繼續使用MarsEdit當作目前的方案
如果真的遇到無法解決的問題，灌VirtualBox弄個Win7來寫Blog也是有可能的。




 




以下是整理Three20的初步體驗心得：






	
  * 下載Three20 程式碼 [http://three20.info/roadmap/1.0.6.2](http://three20.info/roadmap/1.0.6.2)

	
  * 在Xcode建立一個View base App

	
  * 使用終端機輸入以下指令去把Framework加入

	
    * 

    
    python three20/src/scripts/ttmodule.py -p Three20Test1/Three20Test1.xcodeproj Three20 --xcode-version=4




	
    * 注意相關的路徑




	
  * 打開專案會看到相關Framework 加好了，接下來你需要的是找一些範例來展示Three20的火力


參考以下網頁找範例：

	
  * [http://www.haogongju.net/art/1473534](http://www.haogongju.net/art/1473534)

	
  * [http://www.xuanyusong.com/archives/652](http://www.xuanyusong.com/archives/652)

	
  * [http://c.gzl.name/archives/tag/three20](http://c.gzl.name/archives/tag/three20)



