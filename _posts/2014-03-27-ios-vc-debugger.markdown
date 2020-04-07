---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-27 11:01:57+00:00
slug: iosvc-%e9%97%9c%e6%96%bcdebugger-%e7%9a%84%e9%80%b2%e9%9a%8e%e5%88%a9%e7%94%a8-%e5%88%a9%e7%94%a8breakpoint%e5%b9%ab%e4%bd%a0%e5%88%97%e5%8d%b0%e4%b8%80%e4%ba%9b%e8%b3%87%e8%a8%8a
title: '[iOS][VC] 關於debugger 的進階利用 - 利用breakpoint幫你列印一些資訊...'
wordpress_id: 1365
categories:
- iOS的程式開發
- VC++程式設計
---

最近看了一篇[挺好的文章(快速開發iOSApp)](http://www.bradleylin.net/blog/speed-up-your-ios-development)，雖然是iOS的文章但是有提到一段是關於XCode debugger的進階運用教學  
當然這裡提到的不是hit count，condition 那一系列的用法．  
這裡有影片，可能需要iOS Dev 帳號 ([影片1](https://developer.apple.com/videos/wwdc/2012/?include=402#402)) ([影片2](https://developer.apple.com/videos/wwdc/2012/?include=412#412))




裡面最好用(最近)的就是直接利用break point 去印一些文字幫助你debug






  * 設立一個breakpoint


  * 使用右鍵Edit breakpoint


  * “Add action”-> 印出你需要的物件(po XXX 或是  expr (void)NSLog(@“%f”, XXX)


  * 點選”Automatically continue after evaluating”




這裡有[官方文件](https://developer.apple.com/library/mac/recipes/xcode_help-breakpoint_navigator/articles/setting_breakpoint_actions_and_options.html#//apple_ref/doc/uid/TP40010433-CH3-SW1)，不過還是影片教學比較好．




回過頭來也去研究微軟的Vistual Studio 看看有沒有類似的功能．沒有那麼強大～但是其實也是有的．以下是VS2013的設定步驟:






  * 建立一個Breakpoint


  * 一樣使用右鍵，選取”When Hit”


  * 印出你要的字，這裡如果要印出變數～多做一些功...  




這裡有[完整的說明](http://msdn.microsoft.com/en-us/library/5557y8b4.aspx#BKMK_Print_to_the_Output_window_with_tracepoints)，可以參考...




**兩者(Xcode 5 V.S. VS2013 SP1) 比較一下**






  * VS2013



    * 只可以印出資訊 (參考 [DebuggerDisplayAttribute](http://msdn.microsoft.com/en-us/library/system.diagnostics.debuggerdisplayattribute.aspx) ) 可以直接“{變數}”來列印



  * XCode



    * 不僅僅可以列印變數，影片中甚至建議你可以利用這個去新增或是變更你的breakpoint.


    * 基本上 Add Action 所以任何Action都可以思考．



      * 影片中有提到可以使用 AppleScript 去寄信給你當任何exception 發生的時候．這樣一來可以跑overnight testing 隔天再來看結果就好了．



    * 但是如同前面提到的，需要跑一些特殊的動作的時候．確認不會變動到你的邏輯．





**使用這個的好處是什麼？ **






  * 不用改code，不用一個個breakpoint 停下來看．馬上可以了解程式的流程....


  * 不用切換到debug version，可以直接在release來利用印出文字了解流程，但是release版本無法正確印出變數（除非是managed code)． 很多問題都只發生在release version你懂得． (尤其是客戶手上的那版 XD)




**但是其實也是有一些缺點:**






  * 變得慢.. 別忘了任何的breakpoint 都是在程式內部增加 interrupt 所以一定變慢（VS2013上還挺明顯的)


  * 當你試著要做某些動作的時候，有可能會變更IDE內部的動作．這個在XCode的演講裡面有提到，可以詳細去看...


