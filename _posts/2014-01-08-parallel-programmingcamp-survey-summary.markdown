---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-01-08 15:21:55+00:00
slug: parallel-programmingcamp-survey-summary
title: '[Parallel Programming][C++AMP] Survey summary'
wordpress_id: 1289
categories:
- VC++程式設計
- 學習文件
---

**Basic requirement:**






  * VS2012 runtime


  * DX11 runtime


  * Use test program "[VerifyAmpDevices](http://www.danielmoth.com/Blog/verifyampdevices.exe)" to verify it




**Advantage**:






  * Compatible with any GPU which could run DX11 with one EXE.



    * NVIDIA GPUs


    * AMD GPUs (and APUs)


    * Intel GPUs (Ivy Bridge and later)


    * ARM GPUs from various IHVs (soon, e.g. see Mali design)



  * Could use Visual Studio IDE for GPU threading debugging


  * Could use Visual Studio IDE for GPU profiling


  * Base on C++ and STL




**Performance Comparison: (refer **[here](http://codinggorilla.domemtech.com/?p=1135)**)**




**  
** **Video Clip:**  





  * [http://channel9.msdn.com/Events/AMD-Fusion-Developer-Summit/AMD-Fusion-Developer-Summit-11/KEYNOTE](http://channel9.msdn.com/Events/AMD-Fusion-Developer-Summit/AMD-Fusion-Developer-Summit-11/KEYNOTE) AFDS keynote which also introduce C++ AMP concept.


**Code Resource:**  





  * Hello world with C++ AMP [http://blogs.msdn.com/b/nativeconcurrency/archive/2012/04/30/hello-world-using-textures-in-c-amp.aspx](http://blogs.msdn.com/b/nativeconcurrency/archive/2012/04/30/hello-world-using-textures-in-c-amp.aspx)


  * All C++ AMP samples: [http://blogs.msdn.com/b/nativeconcurrency/archive/2012/01/30/c-amp-sample-projects-for-download.aspx](http://blogs.msdn.com/b/nativeconcurrency/archive/2012/01/30/c-amp-sample-projects-for-download.aspx)


  * Very simple code sample slide: [http://ecn.channel9.msdn.com/content/DanielMoth_CppAMP_Intro.pdf](http://ecn.channel9.msdn.com/content/DanielMoth_CppAMP_Intro.pdf)


  * Detail analysis article [http://kheresy.wordpress.com/2011/06/16/c-accelerated-massive-parallelism/](http://kheresy.wordpress.com/2011/06/16/c-accelerated-massive-parallelism/)


  * Limitation on C++ AMP [http://blogs.msdn.com/b/nativeconcurrency/archive/2012/02/07/double-precision-support-in-c-amp.aspx](http://blogs.msdn.com/b/nativeconcurrency/archive/2012/02/07/double-precision-support-in-c-amp.aspx)


  * 


    * WDDM 1.1, there will be double precision problem, only resolve on WDDM 1.2 (Win8)






