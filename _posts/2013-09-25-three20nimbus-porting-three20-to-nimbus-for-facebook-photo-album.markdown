---
layout: post
layout: post
author: kkdai
comments: true
date: 2013-09-25 13:53:17+00:00
slug: three20nimbus-porting-three20-to-nimbus-for-facebook-photo-album
title: '[Three20][Nimbus] Porting Three20 to Nimbus for Facebook Photo Album'
wordpress_id: 1278
categories:
- iOS的程式開發
---

This article is a record to summarized my project to migration from Three20 to Nimbus.




Here is specific step by step to help you porting (migrate) your three20 




Original project is a Facebook photo album which implement base on [http://www.raywenderlich.com/1430/three20-tutorial-for-ios-how-to-use-the-three20-photo-viewer](http://www.raywenderlich.com/1430/three20-tutorial-for-ios-how-to-use-the-three20-photo-viewer)






  * Add Nimbus in your project



    * Sync code



      * git clone https://github.com/jverkoey/nimbus.git


      * Goes to nimbus path


      * git submodule init


      * git submodule update



    * Include nimbus code in project



      * Core


      * Photos


      * Paging Scroll View


      * Models


      * Overview



    * Include Thirdparty in your code



      * JSONKit



        * remember to add "-fno-objc-arc" on Targets -> Build Phase -> Compiling Phase to disable ARC compiling



      * AFNetworking



    * Add framework



      * libz.dylib


      * MobileCoreServices.framework


      * SystemConfiguration.framework


      * CFNetwork.framework



    * Add those three include in your pch


    * 


    #import "NimbusCore.h"




    #import "NimbusPhotos.h"




    #import "NimbusModels.h"






  * Remove original Three20 project



    * Remove all Three20 xproj in your framework.



  * Replace using to replace TTPhotoViewController to FacebookPhotoAlbumViewController



    * Remove photo.h Photo.m


    * Remove PhotoSet.h PhotoSet.m


    * Remove PhotoViewController.h PhotoViewController.m


    * Drag NimbusPhotos.bundle from src/photos/resources into your project.


    * Add follow file into your project



      * NetworkPhotoAlbumViewController.m NetworkPhotoAlbumViewController.h


      * FacebookPhotoAlbumViewController.m FacebookPhotoAlbumViewController.h


      * CaptionedPhotoView.m CaptionedPhotoView.h



    * Replace PhotoSource related code to follow direct using album ID



      * 


    LikePersonAlbum *myEntity = [_fetchResultControllerobjectAtIndexPath:indexPath];




 




    Class vcClass = [FacebookPhotoAlbumViewControllerclass];




    id initWith = _Facebookalbum_id;




    NSString* title = @"test1";




    UIViewController* vc = [[vcClass alloc] initWith:initWith];




    vc.title = title;




    [self.navigationControllerpushViewController:vc animated:YES];







  * Done, less than 2 hours.




**Advantage**






  * Fast, easy to porting your code to source version control without any setting.


  * Compiler time reduce a lot. (Really~~~  a lot)




**TODO:**









  * According to Nimbus "[Three20 migration Guide](http://wiki.nimbuskit.info/Three20-Migration-Guide)",  we should use NIToolbarPhotoViewController. Will check it later.



