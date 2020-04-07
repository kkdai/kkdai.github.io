---
layout: post
layout: post
author: kkdai
comments: false
date: 2013-02-01 02:07:21+00:00
slug: iosnumbusthree20-%e9%97%9c%e6%96%bcphoto-viewer%e5%ad%b8%e7%bf%92%e5%bf%83%e5%be%97
title: '[iOS][Nimbus][Three20] 關於Photo Viewer學習心得~'
wordpress_id: 1222
categories:
- iOS的程式開發
- 學習文件
---

會想學這個～主要是想找一個iOS上面對於相簿處理比較漂亮的範例程式碼
本來在找的是[Three20](http://three20.info/)，但是在[Three20](http://three20.info/)官方網站有在推薦[nimbus
](nimbuskit.info)所以也試著把它弄起來～

[nimbus](nimbuskit.info)安裝流程:



	
  * 根據他的安裝流程，發現沒裝git

	
  * 先到google上的[git Mac install](http://code.google.com/p/git-osx-installer/) 去安裝

	
  * 如果直接在[nimbus](nimbuskit.info) 下載source code來使用會發現一堆third party error 像是 "Afnetworking.h file not found"

	
  * 所以要依照他的方式來安裝：

	
    * git clone https://github.com/jverkoey/nimbus.git




	
  * 接下來要到你下載的目錄(應該是nimbus)去更新他的相關的其他程式碼

	
    * git submodule init

	
    * git submodule update




	
  * 這樣就可以更新到其他的相關專案[Afnetworking](https://github.com/AFNetworking/AFNetworking/) 跟[JSONKit](https://github.com/johnezang/JSONKit)

	
  * 接下來如何使用可以參考 [http://wiki.nimbuskit.info/Add-Nimbus-to-your-project](http://wiki.nimbuskit.info/Add-Nimbus-to-your-project)

	
  * Photo Viewer範例可以在sample 找到～但是似乎先是為了網路相簿～  要在看一下




[Three20 PhotoViewer]




不過看了一下～ 似乎sample還是無法馬上使用，看來再回去找 [Three20](http://three20.info/)看看有沒有快速解決方案




找到有人放上[Three20](http://three20.info/) Photo Viewer的教學([這裡](http://www.raywenderlich.com/1430/how-to-use-the-three20-photo-viewer))




下載下來～加上把[Three20](http://three20.info/)導入～馬上就能用








	
  * python three20/src/scripts/ttmodule.py -p PhotoViewer/PhotoViewer.xcodeproj Three20 --xcode-version=4





嗯～  可能要修一些在Three20專案內的compiler error ~ 先comment 掉算了～
其他細節可以在[http://www.raywenderlich.com/1430/how-to-use-the-three20-photo-viewer](http://www.raywenderlich.com/1430/how-to-use-the-three20-photo-viewer) 找到

不過考量到我需要快速開發存取Facebook 相簿的程式～還是先使用[nimbus
](nimbuskit.info)

詳細流程如下：






	
  * 打開iOS default view project with ARC setting

	
  * 新增 new Group 把以下部分的src 都放入～注意不要copy 過去(只要加入src就好～多加上example可能會出錯）

	
    * Photos

	
    * Paging Scroll View

	
    * Overview

	
    * Models

	
    * Core




	
  * 加入相關的framework

	
    * libz.dylib

	
    * MobileCoreServices.framework

	
    * SystemConfiguration.framework

	
    * CFNetwork.framework




	
  * 把檔案加入pch

	
    * **#import ****"NimbusCore.h"**

	
    * **#import ****"NimbusPhotos.h"**

	
    * **#import ****"NimbusModels.h"**




	
  * 出現compiler error

	
    * `#import <SenTestingKit/SenTestingKit.h>`) could not find

	
    * 解決法 加入 ${DEVELOPER_LIBRARY_DIR}/Frameworks 在Framework Search Paths [參考](http://stackoverflow.com/questions/3354891/xcode-sentestingkit-not-found)




	
  * 接下來可能要利用到 [nimbus](nimbuskit.info)  裡面專案的檔案~作為FB album 測試

	
    * CaptionedPhotoView.h

	
    * CaptionedPhotoView.m

	
    * FacebookPhotoAlbumViewController.h

	
    * FacebookPhotoAlbumViewController.m

	
    * NetworkPhotoAlbumViewController.h

	
    * NetworkPhotoAlbumViewController.m




	
  *   接下來的部分就是把其他的部份依照[http://latest.docs.nimbuskit.info/NimbusPhotos.html](http://latest.docs.nimbuskit.info/NimbusPhotos.html)的範例來增加～不詳細敘述。







參考資料：








	
  * Nimbus 官方

	
    * [http://latest.docs.nimbuskit.info/NimbusPhotos.html](http://latest.docs.nimbuskit.info/NimbusPhotos.html)

	
    * [http://wiki.nimbuskit.info/Add-Nimbus-to-your-project](http://wiki.nimbuskit.info/Add-Nimbus-to-your-project)

	
    * [http://wiki.nimbuskit.info/Three20-Migration-Guide](http://wiki.nimbuskit.info/Three20-Migration-Guide)




	
  * Three20

	
    * [http://www.raywenderlich.com/1430/how-to-use-the-three20-photo-viewer](http://www.raywenderlich.com/1430/how-to-use-the-three20-photo-viewer)




	
  * Nimbus  相關中文

	
    * [http://www.cocoachina.com/newbie/tutorial/2012/1114/5080.html](http://www.cocoachina.com/newbie/tutorial/2012/1114/5080.html)

	
    * [http://blog.csdn.net/youngshook/article/details/7336711](http://blog.csdn.net/youngshook/article/details/7336711)

	
    * [http://code4app.com/search/Nimbus](http://code4app.com/search/Nimbus)


















