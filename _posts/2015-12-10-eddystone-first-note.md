---
layout: post
layout: post
title: "[Golang][BLE] Eddystone 初體驗(包含Beacon模擬器)"
description: ""
category: 
- golang
tags: [ble, eddystone]
---

![](https://lh3.googleusercontent.com/gcEhJFg9o3geDddamHn-HQthPyaYhyyWxAC7nY2E6LI-bBCkHeLsPqB7ZpebwAFXsDvlx9V66Jqw98YlTy6DxeE4soDA7Q=s888)

## 前言

今年(2015)的年中，Google發表了[Beacon Platform](https://developers.google.com/beacons/)，並且引入了[Eddystone](https://github.com/google/eddystone)也就是新的Beacon資料格式．本篇整理包含了簡單的資料格式介紹，Beacon模擬器的尋找(我也寫了一個模擬器)與一些App去掃描Eddystone Beacon．


## 那麼來開始看Eddystone

Eddystone 主要的資料單位是`Frame`，而主要有三種資料為:

- [URL Frame](https://github.com/google/eddystone/tree/master/eddystone-url):
	- 用來顯示URL，裡面的資料會經過編碼．
- [UID Frame](https://github.com/google/eddystone/tree/master/eddystone-uid):
	- 用16 bytes來記錄一個唯一的識別碼，其中包含10 bytes的namespace跟6 bytes的instance
- [TLM Frame](https://github.com/google/eddystone/tree/master/eddystone-tlm):
	- 這個Frame主要是拿來傳輸資料，主要是用於傳輸beacon的感知器資料（電池壽命，溫度，開機多久...)．


## 啟動一個Eddystone Beacon Simulator (模擬Eddystone Beacon) 

這邊有兩個語言推薦，不過主要都是支援Mac OS與Linux．大家可以看看自己決定要不要使用．

- Node.js - Eddystone beacon simulator
	- [Eddystone beacon simulator](https://github.com/don/node-eddystone-beacon)
	- 如何跑的細節可以看接下來的教學．

- Golang - suapapa/go_eddystone
	- [https://github.com/suapapa/go_eddystone](https://github.com/suapapa/go_eddystone)
	- 執行: (Mac OSX)
		- `go get github.com/suapapa/go_eddystone`
		- `go run $GOPATH/src/github.com/suapapa/go_eddystone/example/beacon.go`

### Node.js Eddystone Simulator與簡單Node.js教學

這邊稍微紀錄一下關於Node.js的簡單教學．會需要用到Node.js主要是因為現在只有支援”[Eddystone beacon simulator](https://github.com/don/node-eddystone-beacon)“這個模擬器，所以得來稍微跑一下Node.js，只要會安裝跟執行就好．

#### 安裝npm

請愛用brew

```
brew install npm
```


#### npm 下載套件

其實npm的套件都是放在網路repository上，所以要裝任何套件只要

	npm install 套件名稱

就可以，但是如果找不到索引，就可能要下載git後本地端安裝

	npm install -l

####	執行Node.js 套件

安裝好套件後，就找到某個.js檔案（可能是自己寫，可能是放在examples)

```
vu app.js
```
加入以下的程式碼

```
EB = require('eddystone-beacon');
EB.advertiseUrl('http://goo.gl/uagFfW');
```

存檔後執行

```
node app.js
```

這樣就可以了..		由於這個套件沒有網頁顯示．執行玩得直接拿Eddystone 的verifier來驗證．

###  如何驗證 Eddystone Beacon是否正確 (Eddystone Validator/Scanner)

#### iOS 可以使用 Chome iOS 版本的 Today

只要進入 Today (iOS 下拉顯示"今天")，加入Chrome Extension． 就會搜尋實體網路 (Physical Web)，可以搜尋到具有 URL Frame 的Eddystone Beacon．

如果要搜尋其他的Eddystone Beacon(UID Frame and TLM Frame_還是要使用專用軟體． 

#### Android 推薦App

這個App我覺得很好用，[iBeacon & Eddystone Scanner](https://play.google.com/store/apps/details?id=de.flurp.beaconscanner.app&hl=zh_TW)． 推薦給大家，還可以查詢RSSI的強弱．


#### Windows 請用Bluetooth Beacon Interactor

(Bluetooth Beacon Interactor)是UAP，[下載鏈結](https://www.microsoft.com/zh-tw/store/apps/bluetooth-beacon-interactor/9nblggh1z24k)．

這裡有[原始碼](https://github.com/andijakl/universal-beacon)

## 相關專案

最後我把所有的Golang beacon simulator 整理成一個小專案，希望能幫助大家．

[https://github.com/kkdai/beacon](https://github.com/kkdai/beacon)

[更新:12/30] 我也寫了一個Golang的[Eddystone Beacon Scanner](https://github.com/kkdai/EddystoneScanner)有興趣可以看看． 

## 相關鏈結

- [Go for Eddystone translator](https://github.com/suapapa/go_eddystone)
- [Eddystone beacon simulator](https://github.com/don/node-eddystone-beacon)
- How to discover/verify Eddystone beacon
	- [iOS - Use Chrome App Today extension](https://docs.pushmote.com/docs/how-to-enable-chromes-physical-web-extension-on-the-ios)
- [Eddystone simulator: HOWTO](http://qiita.com/Morikuma_Works/items/b8c22a6e8821e3b29530)
- [Google Beacon OS的教學](https://github.com/google/beacon-platform)
- [Google Eddystone](https://github.com/google/eddystone)
- Beacon Scanner App:
	- iOS Chome
	- Android [iBeacon & Eddystone Scanner](https://play.google.com/store/apps/details?id=de.flurp.beaconscanner.app&hl=zh_TW)
	- Windows 10 [Bluetooth Beacon Interactor](https://www.microsoft.com/zh-tw/store/apps/bluetooth-beacon-interactor/9nblggh1z24k)
