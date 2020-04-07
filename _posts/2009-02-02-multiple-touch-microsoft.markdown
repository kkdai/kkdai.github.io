---
layout: post
layout: post
author: kkdai
comments: true
date: 2009-02-02 10:11:28+00:00
slug: multiple-touch-microsoft
title: Multiple-touch Microsoft
wordpress_id: 1018
categories:
- 下一代視窗系統心得
---

Please refer [http://channel9.msdn.com/pdc2008/PC03/](http://channel9.msdn.com/pdc2008/PC03/) for more detail.

 

![Multi-Touch.jpg](http://farm4.static.flickr.com/3122/3209597398_fea85ee8cd.jpg)

 

**API analysis:**

 

  
  1. GOOD (MultiTouchScratchPadRTS)             
    1. It cold migrate into WPF. 
       
    2. It also the row data no such Gesture API for using. 
       
    3. Just like some raw data for WMTouch. 
       
    4. MS Sample: [下載](http://www.badongo.com/file/13188090)
       
   
  2. Better (MultiTouchGesture)             
    1. Only work on Win7 SDK. 
       
    2. Only some Gesture for using (Zoom/Rotate ...) 
       
    3. Work with C++, might need some work for WPF. 
       
    4. MS Sample: [下載](http://www.badongo.com/file/13188086)
       
   
  3. Best (WMTouch)             
    1. Raw data no other API for it. 
       
    2. MS Sample: [下載](http://www.badongo.com/file/13188047)
       
 

**Working machine and driver**:

 

  
  * OS             
    * Win7 B6801 beta 
       
   
  * Machine:             
    * Dell Latitude XT (Over two point support) 
       
    *        
   
  * Driver:             
    * DuoSense_MT_PreBetaForWin7_V4.exe from [http://www.n-trig.com/](http://www.n-trig.com/)
       
 

**Summary**:

 

Multiple touch depend on Target machine, OS and driver. Especially the driver and OS. Sometime we found the driver may failed after some instruction. It is greate for any app to support multiple-touch, but how to work-out a greate user behavior is harder than migration.

 

**Reference**:

 

  
  * [http://en.wikipedia.org/wiki/Multi-touch](http://en.wikipedia.org/wiki/Multi-touch) WIKI 
   
  * [http://www.n-trig.com/](http://www.n-trig.com/)
