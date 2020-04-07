---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-23 02:44:37+00:00
slug: ios-%e7%b2%89%e7%b5%b2%e7%9b%b8%e7%b0%bf%e6%9b%b4%e6%96%b0%e5%88%b0-1-1-%e6%94%af%e6%8f%b4%e7%b6%b2%e5%8f%8b%e5%88%86%e4%ba%ab%e8%88%87%e8%a9%95%e5%88%86
title: '[iOS] 粉絲相簿更新到 1.1 支援網友分享與評分'
wordpress_id: 1463
categories:
- App-粉絲相簿
- iOS的程式開發
---

六月底上架之後，雖然一開始是用收費的方式，主要還是發給一些親朋好友測試而已．  
收集了各方意見我也增加了新的功能之後，在今天就要上架更新的版本．




[https://itunes.apple.com/tw/app/fen-si-xiang-bu/id839324997?l=zh&mt=8](https://itunes.apple.com/tw/app/fen-si-xiang-bu/id839324997?l=zh&mt=8)




![](https://raw.githubusercontent.com/kkdai/iOS-APP-FBAlbums/master/img/1.1/icon.png)




 




#### 1.1 版本的新內容




- 新增了網友推薦的功能，你可以分享其他網友  
- 新增評分功能，可以表示你喜歡這個粉絲頁面  
- 新增訊息通知，可以讓你知道更多受人喜愛的粉絲頁面




有任何問題，請到這邊去反映:  [https://github.com/kkdai/iOS-APP-FBAlbums](https://github.com/kkdai/iOS-APP-FBAlbums)




以下是是比較技術方面的更新心得與筆記:






  * 把網路資料從Google Doc改到Parse，並且支援push message


  * 支援網友上傳喜愛的粉絲頁面到主機上面讓大家去看並且評分（目前只支援”加分”)


  * iAD 增加並且把售價將到免費，之後再考慮有沒有付費機制(AIP 其實不好搞)


  * CoreData 的migration 也搞了很久～其實有考慮要不要拿掉CoreData～


  * 上架之後，想不到iAD要另外的審核～看來又得等待了....




接下來下一次的更新應該會著重在穩定度跟網路讀取的改進，這邊也是我現在比較不了解的地方
