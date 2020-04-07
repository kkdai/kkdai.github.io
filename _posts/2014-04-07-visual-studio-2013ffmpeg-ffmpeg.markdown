---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-04-07 09:13:07+00:00
slug: visual-studio-2013ffmpeg-%e5%a6%82%e4%bd%95%e5%9c%a8-vs2013%e4%b8%8a%e9%9d%a2%e4%bd%bf%e7%94%a8-ffmpeg
title: '[Visual Studio 2013][ffmpeg] 如何在 VS2013上面使用 ffmpeg'
wordpress_id: 1371
categories:
- ffmpeg
- VC++程式設計
---

其實相當簡單，不過把一些常遇到的問題記錄一下



	
  * 可以到官方位置去下載 [http://ffmpeg.zeranoe.com/builds/](http://ffmpeg.zeranoe.com/builds/)

	
    * Shared —> DLL檔案需要給執行檔用的

	
    * Dev      —> include/lib 你在編譯的時候需要用到




	
  * 把 include / lib 加入相關的VC設定

	
  * 需要sample code(範例程式 找找 doc/example/avcodec.c)

	
  * 如果你改成cpp以下需要注意:

	
    * 前面需要加上，避免C/C++的衝突


#include <math.h>







extern "C"{




#ifndef __STDC_CONSTANT_MACROS




#define __STDC_CONSTANT_MACROS




#endif







#include <libavutil/opt.h>




#include <libavcodec/avcodec.h>




#include <libavutil/channel_layout.h>




#include <libavutil/common.h>




#include <libavutil/imgutils.h>




#include <libavutil/mathematics.h>




#include <libavutil/samplefmt.h>




}




	
    * 遇到問題snprintf compiler error

	
      * 換成  _snprintf




	
    * 遇到warning C4996: 'sprintf' was declared deprecated

	
      * 加上#pragma warning(disable:4996)




	
    * Release 會遇到 error LNK2026: module unsafe for SAFESEH image. 

	
      * Project Property Page —> [Linker] —> [Advanced]—> [Image Has Safe Exception Handler] ==>YES—> NO







	
  * 記得要把你會用到的lib 加入link 在這裡是 avcodec.lib, avutil.lib

	
  * 最後記得exe需要跟prebuild 出來的部分放在一起




最後放上改好的sample




 




 




參考:




	
    * [http://elvisjeng.blogspot.tw/2011/11/visual-studio-2010-ffmpeg.html](http://elvisjeng.blogspot.tw/2011/11/visual-studio-2010-ffmpeg.html)

	
    * [http://www.cnblogs.com/Jerry-Chou/archive/2011/03/31/2000761.html](http://www.cnblogs.com/Jerry-Chou/archive/2011/03/31/2000761.html)

	
    * [http://stackoverflow.com/questions/2915672/snprintf-and-visual-studio-2010](http://stackoverflow.com/questions/2915672/snprintf-and-visual-studio-2010)



