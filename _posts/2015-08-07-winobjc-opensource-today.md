---
layout: post
layout: post
title: "[新聞與心得]微軟開源winobjc"
description: ""
category: 
- vc++程式設計
- ios的程式開發
tags: []

---


本日最紅的github (不到24h 超過2000+ star)，莫過於微軟的[winobjc](https://github.com/Microsoft/WinObjC/)，玩了一天分析一下:

###先講結論: 

####想使用的話.. 還不用急著用.. 除非你想幫微軟土炮iOS 所有foundation XDDD


1. 首先這是一個把你的iOS App ObjectC的code 透過vsimporter轉出一個 VS solution．然後透過VS2015 的clang compiler 編譯成 Windows Universal App (or Windows Phone App)
2. 你能想到的 UIKit AVFoundation 都是土法透過DX11自幹出每一個功能(reverse engineering)... 所以要開源請大家一起幫忙土炮
3.除了github 上面提到的一狗票還沒成功之外，其實就算是UIKit 也有很多基本的class功能也都沒有土炮出來.. 更複雜的BLE 跟Map當然也都還沒有 (等你幫忙XD，有人說.. [路還很遠](https://github.com/Microsoft/WinObjC/issues/27))
4. License 採取MIT不過不少人看到裡面的程式碼都是copy & paste 很多其他的人的心血 [BigZaphod/Chameleon](https://github.com/BigZaphod/Chameleon) 跟 [mono/CocosSharp](https://github.com/mono/CocosSharp/blob/master/Extensions/GUI/CCControlExtension/CCControlUtils.cs) .. 應該有更多....
5. 根據微軟說的.. 這部分的核心 clang compiler 不會 open source XDDDD


### 參考資料

- [Hacker News](https://news.ycombinator.com/item?id=10018208)
- [MSFT Blog](http://blogs.windows.com/buildingapps/2015/08/06/windows-bridge-for-ios-lets-open-this-up/)
