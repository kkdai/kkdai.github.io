---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-06-23 18:33:02+00:00
slug: ios%e7%ac%ac%e4%b8%80%e5%80%8bios-app%e4%b8%8a%e6%9e%b6-%e7%b2%89%e7%b5%b2%e7%9b%b8%e7%b0%bf-1-0
title: '[iOS]第一個iOS App上架- 粉絲相簿 1.0'
wordpress_id: 1420
categories:
- App-粉絲相簿
- iOS的程式開發
---

被退回來三次，被要求加上教學模式後，人生第一個自己弄的iOS App終於上架．  
以下的鏈結可以下載:  [https://itunes.apple.com/tw/app/fen-si-xiang-bu/id839324997?l=zh&mt=8](https://itunes.apple.com/tw/app/fen-si-xiang-bu/id839324997?l=zh&mt=8)




![](https://raw.githubusercontent.com/kkdai/iOS-APP-FBAlbums/master/img/1.0/1.png)




這裡記錄一下整個心路歷程，排除掉App本身比較困難的部分之外:






  * 一開始其實最困難的其實就是開啟一個App的憑証，並且讓iPhone可以正常的安裝到憑証．（其實是讓Xcode安裝憑証) 



    * 詳細可以參考: [http://app-island.com/app/2560/%E8%A3%BD%E4%BD%9Cxcode%E6%86%91%E8%AD%89%EF%BC%8C%E8%AE%93%E9%96%8B%E7%99%BC%E7%9A%84APP%E5%8F%AF%E4%BB%A5%E5%AE%89%E8%A3%9D%E5%88%B0iOS%E8%A3%9D%E7%BD%AE%E5%81%9A%E6%B8%AC%E8%A9%A6](http://app-island.com/app/2560/%E8%A3%BD%E4%BD%9Cxcode%E6%86%91%E8%AD%89%EF%BC%8C%E8%AE%93%E9%96%8B%E7%99%BC%E7%9A%84APP%E5%8F%AF%E4%BB%A5%E5%AE%89%E8%A3%9D%E5%88%B0iOS%E8%A3%9D%E7%BD%AE%E5%81%9A%E6%B8%AC%E8%A9%A6)



  * 可以安裝App到手機後，之後困難的就是上傳的問題還有Provisioning Profiles的簽署


  * 好不容易上傳上去後，接下來就是等待審查


  * 第一次失敗:  有兩個問題



    * FB Login 會跳出網頁，也就是我的 Single Sign On 沒弄好


    * 要求增加分享的功能



  * 修復好SSO之後並且增加了分享功能後又送上去審查，又被打回來



    * UI需要有更清楚的標示．



  * 經過解釋後把整個說明網頁弄得清楚一點之後，又送上去．結果第三次被打回來：



    * 還是覺得不夠清楚，雖然我在solution center裡面有解釋整個流程，但是審查人員希望可以有tutorial告訴使用者．



  * 這時候花了比較多的時間，問了一些人有沒有比較好的tutorial 的SDK可以用．有人推薦了這個[WSCoachMarkView](https://github.com/workshirt/WSCoachMarksView)．真的算簡單，然後方便的．


  * 總算通過審查，但是仍然久久沒上架去看了一下後，發現遇到 Pending Contract，原來是銀行跟稅務沒有設定好．參考[這裡](http://kirenenko-tw.blogspot.tw/2013/01/ios-app.html)


  * 最後就會出現Wait for Sale，再過沒多久就出現啦．


  * 最後分享一下如何分享你的App網址給人家


  * 到iTune 裡面尋找你的App，在圖示上面按下右鍵複製鏈結就可以了．


  * 想要更多關於我App粉絲相簿的介紹？去 Github看吧  [https://github.com/kkdai/iOS-APP-FBAlbums](https://github.com/kkdai/iOS-APP-FBAlbums)




參考:






  * 關於[憑証安裝](http://app-island.com/app/2560/%E8%A3%BD%E4%BD%9Cxcode%E6%86%91%E8%AD%89%EF%BC%8C%E8%AE%93%E9%96%8B%E7%99%BC%E7%9A%84APP%E5%8F%AF%E4%BB%A5%E5%AE%89%E8%A3%9D%E5%88%B0iOS%E8%A3%9D%E7%BD%AE%E5%81%9A%E6%B8%AC%E8%A9%A6)


  * [[iOS][FacebookSDK] 更換Facebook SDK 到 3.13啟動 SSO (Single Sign On)](http://www.evanlin.com/blog/?p=1360)


  * [[iOS] 上架App到Apple去review前會遇到的一些問題– 使用 XCode Organizer 來遞交App到Store](http://www.evanlin.com/blog/?p=1350)


  * [IOS App上架經驗分享(三)](http://kirenenko-tw.blogspot.tw/2013/01/ios-app.html)


