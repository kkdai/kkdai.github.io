---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-05-20 01:46:09+00:00
slug: live555openrtspsdlffmpeg-%e5%88%a9%e7%94%a8ffmpeg-%e8%88%87sdl-%e9%81%94%e6%88%90-streaming-%e7%ad%86%e8%a8%981
title: '[live555][OpenRTSP][SDL][ffmpeg] 利用ffmpeg 與SDL 達成 streaming 筆記(1)'
wordpress_id: 1405
categories:
- ffmpeg
- RTSP
- VC++程式設計
---

這邊記錄一下筆記，因為太多公司內部的運作就無法貼code






  * 關於ffmpeg Streaming與 SDL



    * ffmpeg本身就支援 RTSP streaming，只是你需要把資料用 SDL來render


    * 這邊可以參考這段tutorial 



      * [http://dranger.com/ffmpeg/](http://dranger.com/ffmpeg/)


      * 中文解釋(1) [http://ycfu.blog.mypc.tw/2010/01/ffmpegsdl-1.html](http://ycfu.blog.mypc.tw/2010/01/ffmpegsdl-1.html)




  * 從live555的 OpenRTSP 拿出 data buffer 丟到 ffmpeg 去decode



    * 這邊大概是我最不熟悉的部分，也是花最久時間的部分．一開始是拿openRTSP.c 來改


    * 由於我是接H264得source stream所以我拿 QuickTimeFileSink來用，選定MPEG4與MP4之後，其實檔案接下來都可以直接播放～這是沒有問題的．


    * 那如何要把資料接出來，方法如下:



      * 先到QuickTimeFileSink的 AfterGettingFrame 去implement 一個callback function


      * 其中最重要的是把buffer data 接出來



        * ioState->fBuffer->dataStart 這是frame buffer資料的起頭


        * packageDataSize 是frame buffer的大小



      * 由於資料是一個video frame 後面緊接著有數個audio frame，所以記得要先查看是哪一種格式好做相關的decode 準備



        * ioState->fOurSubsession.mediumName()



      * 至於接出來要怎麼decode 就要參考第一段的tutorial


      * 這裡要注意的是，由於拿到的frame data 是一張一張的，所以許多ffmpeg資料要填好 (SDL_AudioSpec)




  * 在ffmpeg中audio decoder無法順利地打開



    * 這個問題主要是在SDL_OpenAudio 可以正常開起，如果有把相關資料填對的話．但是卻無法開啟ffmpeg audio 的 avcodec_open2 


    * 原因是因為你需要把 audio Codec context 要填對，主要是:



      * adoContext->sample_rate


      * adoContext->channel




  * 在ffmpeg 接到的時候，video frame 無法順利decode



    * 這個狀況會很詭異的是，直接用ffmpeg 去接RTSP會成功，但是把一張張frame data接過來用ffmpeg去decode的話就會失敗．


    * 回去觀察原來的h264 frame data會發現，其實在video frame data buffer 起始的四個data 分別是



      * 0x00, 0x00, 0x00, 0x01



    * 所以如果把這四個位元加在video frame buffer的前面，就可以正常decode





 




先寫到這裡，其實還有許多更多的相關處理．之後整理一下心得在寫.... 




 




**參考:**






  * RTSP Client use OpenRTSP (live555) with H264/MJpeg [http://blog.xuite.net/antony0604/blog/130505326-RTSP+Client+use+OpenRTSP+(live555)+with+H264%2FMJpeg%3E](http://blog.xuite.net/antony0604/blog/130505326-RTSP+Client+use+OpenRTSP+(live555)+with+H264%2FMJpeg%3E)



    * 這篇很有幫助，幫助我知道哪些地方開始修改，也知道哪些地雷可以避免踩到



  * ffmpeg(2)：ffmpeg结合SDL2.0解码视频流 [http://blog.csdn.net/oldmtn/article/details/20284721](http://blog.csdn.net/oldmtn/article/details/20284721)



    * 這篇可以幫助你快速建立 ffmpeg + SDL 2.0 player也可幫助我看如何從 SDL 1.2 -> SDL 2.0



  * Live555官方文件  [http://www.live555.com/liveMedia/#testProgs](http://www.live555.com/liveMedia/#testProgs)



    * 最主要這邊是提醒我們可以如何開始把frame buffer 找出來，也提醒我們如何去結束一個session ．主要都是參考  testRTSPClient.c



  * Live555类结构  [http://www.cppblog.com/tx7do/archive/2013/09/10/203134.html](http://www.cppblog.com/tx7do/archive/2013/09/10/203134.html)



    * 把相關的結構整理了一下，其實很適合閱讀．不過呢～這種東西沒有真的下去碰過好像也是都看不懂．



  * live555 livemedia库结构分析[原创]  [http://blog.csdn.net/gjgsoft/article/details/7705349](http://blog.csdn.net/gjgsoft/article/details/7705349)



    * 有把 Medium，Sink與其他的關係作了一下說明，並且有把TestRSTPClient 做一個簡單的講解... 挺值得一看...



