---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-07 07:47:27+00:00
slug: ios-%e9%97%9c%e6%96%bc%e4%b8%80%e4%ba%9b%e7%b6%b2%e8%b7%af%e4%b8%8asdk%e7%9a%84%e5%ad%b8%e7%bf%92%e6%95%b4%e7%90%86%e5%bf%83%e5%be%97
title: '[iOS] 關於一些網路上SDK的學習整理心得'
wordpress_id: 1346
categories:
- iOS的程式開發
---

整理一下最近的關於iOS一些SDK學習心得






  * **FGallery ([https://github.com/gdavis/FGallery-iPhone](https://github.com/gdavis/FGallery-iPhone))**



    * 自從換掉[Three20](https://github.com/facebook/three20)與 [Nimbus](https://github.com/jverkoey/nimbus) 之後，相簿處理的部分就換到這個SDK．其實不錯用只是對於新版本iOS相容性差了點．


    * 優點:



      * 其實整合相當的方便，而且完整相簿得捲動支援(Album scroll view)



    * 缺點:



      * iOS7 之後，完全一整個不穩定，Git上面也一堆使用者反應．不過作者看起來不maintain 了．



    * 替代方案:



      * iOS7之後可能還是換成iOS 預設提供的 UICollectionView功能相當的強大，不僅僅可以作為捲動處理，以後要做為更多的功能我想也不是問題．



        * WWDC 介紹影片: [https://developer.apple.com/devcenter/download.action?path=/videos/wwdc_2012__hd/session_219__advanced_collection_views_and_building_custom_layouts.mov](https://developer.apple.com/devcenter/download.action?path=/videos/wwdc_2012__hd/session_219__advanced_collection_views_and_building_custom_layouts.mov)


        * 參考介紹文章: [http://www.raywenderlich.com/22324/beginning-uicollectionview-in-ios-6-part-12](http://www.raywenderlich.com/22324/beginning-uicollectionview-in-ios-6-part-12)





  * **Nimbus ([https://github.com/jverkoey/nimbus](https://github.com/jverkoey/nimbus))**



    * 這個就不詳細介紹了，提供大部分所有app會使用到的相關功能．原開發就是知名的Facebok UI SDK [Three20](https://github.com/facebook/three20)．


    * 最近忽然開始積極更新版本，很久沒繼續使用了．看來會找時間用用看從頭來兜出一個App



  * **MagicalRecord ([https://github.com/magicalpanda/MagicalRecord](https://github.com/magicalpanda/MagicalRecord))**



    * 這個SDK主要是幫助你處理關於資料的相關處理．CoreData功能很強大，但是一開始的設置跟相關的處理卻相當的麻煩．


    * 而這個不僅僅可以幫助你快速使用CoreData 而不用去管令人討厭的NSManagedObjectContext, NSManagedObjectModel, NSPersistentStoreCoordinator還可以操控不只是CoreData, Plist都可以使用．


    * 如何加入:



      * 把所有檔案加入專案


      * 把這段加到 .pch  


#import "CoreData+MagicalRecord.h"





      * 建立CoreData資料庫


      * AppDelegate 加入  
  [MagicalRecordsetupCoreDataStackWithStoreNamed:@"CoreData.sqlite”];


      * 搞定



    * 詳細流程參考[這篇文章](http://www.cnblogs.com/mybkn/p/3328183.html)



  * **Inappsettingskit ([http://www.inappsettingskit.com/](http://www.inappsettingskit.com/))**



    * 設定App的設定通常都是使用Settings Bundle (參考: [這裡](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html#//apple_ref/doc/uid/10000059i-CH6-SW14))．如果要在app裡面更改設定就得自己都出整個頁面與設定控制，這個SDK可以快速的幫助你建制起來．


    * 流程也簡單到不行:



      * 把InAppSettingsKit資料夾加入你的專案


      * IASKAppSettingsViewController *IASKAppSettingView;


      * 


 if(IASKAppSettingView == nil)




    {




        IASKAppSettingView = [[IASKAppSettingsViewControlleralloc] init];




        IASKAppSettingView.delegate = self;




    }




    UINavigationController *aNavController = [[UINavigationControlleralloc] initWithRootViewController:IASKAppSettingView];




    //[viewController setShowCreditsFooter:NO];   // Uncomment to not display InAppSettingsKit credits for creators.




    // But we encourage you not to uncomment. Thank you!




    IASKAppSettingView.showDoneButton = YES;




    [self  presentViewController:aNavController animated:YEScompletion:nil];




    //   [self SwitchPage:PAGE_SETTING];






    * 中文的介紹文章: [在這裏](http://blog.csdn.net/artwebs/article/details/8295937) 





我是參考[這篇文章](http://blog.csdn.net/kingsley_cxz/article/details/9336229)去找出相關的SDK，把這些SDK都玩過一次之後，又想去翻動我的程式了．一直改來改去~感覺很難上架了~~
