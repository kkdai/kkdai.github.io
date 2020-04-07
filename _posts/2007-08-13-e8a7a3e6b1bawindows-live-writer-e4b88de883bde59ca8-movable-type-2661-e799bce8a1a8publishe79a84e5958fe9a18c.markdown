---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-08-13 13:39:39+00:00
slug: '%e8%a7%a3%e6%b1%bawindows-live-writer-%e4%b8%8d%e8%83%bd%e5%9c%a8-movable-type-2661-%e7%99%bc%e8%a1%a8publish%e7%9a%84%e5%95%8f%e9%a1%8c'
title: 解決Windows Live Writer 不能在 Movable Type 2.661 發表(publish)的問題
wordpress_id: 736
categories:
- 網路上好玩的事情
- 關於MT的學習心得
---

[![WLW.JPG](http://farm2.static.flickr.com/1313/1052177325_9dcd1d232c.jpg)](http://www.flickr.com/photos/27643002@N00/1052177325/)

[Windows Live Writer(WLW)](http://windowslivewriter.spaces.live.com/) 是一個相當好用的軟體，其實之前我的文章"_好用的離線Blog編輯器--Zoundry_"裡面對於這個軟體也是相當的推荐的~~只是無奈我所使用的Movable Type (MT) 2.661 由於一直出現錯誤，他的錯誤如以下的狀況


<!-- more -->
 

。(會出現以下的錯誤訊息)  

<blockquote>发生服务器错误 Client (BLOG Server Error) "Server Error Client Occurred"  
QXBwbGljYXRpb24gZmFpbGVkIGR1cmluZyByZXF1ZXN0IGRlc2VyaWFsaXphd
> 
> GlvbjoganVuayAn77u/JyBiZWZvcmUgWE1MIGVsZW1lbnQK
> 
> </blockquote>

![WLW_errror.JPG](http://farm2.static.flickr.com/1220/935887977_f16bbd1ebc.jpg)  

讓我每次要發表文章~都是先使用其他使我無法使用Destop Blog Editor 編輯過後，在將文章貼上去~~ 這樣實在讓我相當的困擾~ 所以我決定到 [WLW的論壇](http://groups.msn.com/windowslivewriter/generaltopics.msnw?action=get_threads)去尋找解決的方法。 果然讓我很好運的找到一位微軟的工程師來幫助我，他告訴我以下的方法可以解決: (solve the problem)

<blockquote>Run regedit.exe and find the following key: HKEY_CURRENT_USERSoftwareWindows Live WriterWeblogs<your-blog-id>
> 
> Verify that the subkey ManifestOptions contains a characterSet="UTF-8" value. If so, 
> 
> under the subkey UserOptionOverrides, 
> 
> add a new String Value named characterSet and leave its value empty.
> 
> </blockquote>

It's work !!! WOW~~~~ 真的成功了~~~~ 這篇文章就是使用WLW 來發表的~~ 希望各位也能解決問題~~~
