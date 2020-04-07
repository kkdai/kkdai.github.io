---
layout: post
layout: post
title: "[iOS] 把舊專案換到XCode7與iOS9上面會出現上下黑條(Application appears with black bars on top and bottom)"
description: ""
category: 
- ios的程式開發
tags: [iOS_Programming]
---

##問題:

主要是心血來潮把自己就的App"[粉絲相簿](https://itunes.apple.com/tw/app/fen-si-xiang-bu/id839324997?l=zh&mt=8)"想拿出來改版(終於)． 結果發現上下邊有黑條．這裡有個示意圖:

![](http://i.stack.imgur.com/4S93T.png)

不過，我的App明明就是使用系統預設的UITableViewController，NavigationController跟UITabBar而已． 趕緊來找找解決方法．

##主要原因:

根據[iOS 9 Xcode 7 - Application appears with black bars on top and bottom](http://stackoverflow.com/questions/32641240/ios-9-xcode-7-application-appears-with-black-bars-on-top-and-bottom)這邊的解答，是說由於你沒有" LaunchScreen.storyboard"在你的App，所以XCode會"預設"而把你的App變成比較能夠跑的狀況(類似iPhone4 App跑在 iPhone6 Plus一樣)．

這樣其實很不好，因為有上下邊黑條還有擴大的效果．

##解決方式:

根據[http://stackoverflow.com/a/32614526/1316907](http://stackoverflow.com/a/32614526/1316907)這邊的解釋，進行以下的方式可以解決這樣的問題:

- 更改"Launch Image Source" 成 "Brand Assets"
- 不過由於我還是有問題，我必須還要修改"Launch Screen File"成MainStoryBoard

這裡有個修改後示意圖:

![](http://i.stack.imgur.com/Hf9bM.png)


打完收工，本來想加新功能，卻踩一堆舊Bug．不過這樣也學了一課.. 真棒...

##相關鏈結:


- [iOS 9 Xcode 7 - Application appears with black bars on top and bottom](http://stackoverflow.com/questions/32641240/ios-9-xcode-7-application-appears-with-black-bars-on-top-and-bottom)
- [iOS9 App has black bars on top and bottom](http://stackoverflow.com/questions/32312906/ios9-app-has-black-bars-on-top-and-bottom)
- [UITableView under UINavigationBar with a black top background in IOS 9](http://stackoverflow.com/questions/32628982/uitableview-under-uinavigationbar-with-a-black-top-background-in-ios-9/34407989#34407989)
