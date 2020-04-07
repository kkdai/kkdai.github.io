---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-26 01:26:40+00:00
slug: iosfacebooksdk-%e6%9b%b4%e6%8f%9bfacebook-sdk-%e5%88%b0-3-13%e5%95%9f%e5%8b%95-sso-single-sign-on
title: '[iOS][FacebookSDK] 更換Facebook SDK 到 3.13啟動 SSO (Single Sign On)'
wordpress_id: 1360
categories:
- iOS的程式開發
- 學習文件
---

我的iOS App送過去iTune Connect 準備上架的review，經過了一個多禮拜的等待．  
想不到八個小時就失敗了，接下來就得去修改．






  * 要求把Facebook login 從web login 修改成 SSO(Single Sign On)


  * 要求增加一些互動的功能，比如說分享或是Push Notification…




首先先回頭來看我的Facebook SSO login 的部分吧．






  * 首先我發現我的Facebook版本有點舊，於是我去下載並且更新了最新版本3.13


  * 在沒有修改任何code的狀況下，基本上我使用openActiveSessionWithReadPermissions，我發現了以下的問題:



    * 在模擬器上面，由於沒有FB App 所以一定會走到Web login


    * 在手機上面會出現 error code 2~也不會走去web login



  * [再找了許多方式](http://stackoverflow.com/questions/12838118/facebook-authorization-fails-on-ios6-when-switching-fb-account-on-device/22609187#22609187)無法正常解決之後，我決定重新寫一個sample app 來測試．主要是重新看[facebook 這裡的教學](https://developers.facebook.com/docs/ios/getting-started)．想不到就成功了．


  * 於是去分析之後，發現隨著SDK的修改iOS 設定也有以下更改



    * 增加了新數值FacebookDisplayName


    * 由於之前上架，我把我的bunddle name 有修改了，要去FB dev上面的設定去修改



  * 改完之後就可以了.... 想不到換SDK就可以達成SSO，不過可能是bundle name一直錯誤的原因~但是沒有查出來...


  * 如何反覆測試login (How to test SSO repeatedly)



    * 去[Facebook APP]裡面的[隱私設定]->[應用程式] 移除你的App


    * 重新安裝App





提供給大家..




**參考文章:**






  * [http://stackoverflow.com/questions/6786819/how-to-implement-single-sign-on-sso-using-facebook-ios-sdk-for-iphone](http://stackoverflow.com/questions/6786819/how-to-implement-single-sign-on-sso-using-facebook-ios-sdk-for-iphone)


  * [http://n11studio.blogspot.tw/2012/07/ios-facebook-getting-started.html](http://n11studio.blogspot.tw/2012/07/ios-facebook-getting-started.html)


  * [https://parse.com/tutorials/integrating-facebook-in-ios](https://parse.com/tutorials/integrating-facebook-in-ios)


  * [https://developers.facebook.com/docs/reference/iossdk/authentication/](https://developers.facebook.com/docs/reference/iossdk/authentication/)


  * [https://developers.facebook.com/docs/ios/login-tutorial/](https://developers.facebook.com/docs/ios/login-tutorial/)


