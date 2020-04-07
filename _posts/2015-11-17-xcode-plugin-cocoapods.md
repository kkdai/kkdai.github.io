---
layout: post
layout: post
title: "[iOS]安裝XCode的Plugin來操作CocoaPods"
description: ""
category: 
- ios的程式開發
tags: [xcode, objc]

---

![](http://shrikar.com/wp-content/uploads/2015/01/cocoapods.png)

## 前言

其實一直以來有寫iOS的code，不過由於習慣上的因素．一直沒有使用[Cocoapods](https://cocoapods.org/)．  不過最近想說弄個小iPhone App搭著Golang的專案． 所以開始玩，不過[Cocoapods](https://cocoapods.org/)其實一開始沒有那麼容易入手，所以又看了些plugin希望能讓使用上更簡單．

## 原本CocoaPods使用方式:

原本使用方式，需要如下表:

- 安裝CocoaPods `sudo gem install cocoapods`
- 撰寫 `Podfile` (相關[細節](https://guides.cocoapods.org/using/using-cocoapods.html))
- 打上 `pod install`來建立相關必要檔案
- 打開產生的workspace

這其實還挺繁瑣的，所以去找了plugin

## 安裝XCode Plugin 

###先安裝 Plugin Package Manager - Alcatraz

這個Package Manager在安裝的時候，會跳出一個plugin 沒有經過授權的警告，不過就跳過去吧．

安裝[Alcatraz](http://alcatraz.io/):

```
curl -fsSL https://raw.githubusercontent.com/supermarin/Alcatraz/deploy/Scripts/install.sh | sh
```

重新開啟XCode就可以．

###安裝CocoaPods Plugin for Xcode

安裝好[Alcatraz](http://alcatraz.io/)重開之後，打開XCode

		[Windows] -> [Package Manager]
		
裡面選取，[CocoaPods Plugin](https://github.com/kattrali/cocoapods-xcode-plugin) 就可以．
		

安裝好之後，要設定一下`GEM_PATH`，先確認一下你安裝Pod的位置，在Console打上:

```
dirname `which pod`
```

把結果填入 
```
XCode -> [Product] -> [CocoaPods] -> [GEM_PATH]
```

就可以直接在XCode上面更新與修改`Podfile`．

如果還有出現相關問題，可以看這裡[The command path could not be resolved](https://github.com/kattrali/cocoapods-xcode-plugin/issues/76#issuecomment-145579390)
		
## 相關鏈結:

- [Cocoapods](https://cocoapods.org/)
- [XCode Package Manager: Alcatraz](http://alcatraz.io/)
- [CocoaPods Plugin](https://github.com/kattrali/cocoapods-xcode-plugin)
