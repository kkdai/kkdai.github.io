---
layout: post
layout: post
author: kkdai
comments: true
date: 2013-09-18 11:28:28+00:00
slug: iosxcode-%e9%87%8d%e7%81%8cmac-air%e6%89%80%e5%b8%b6%e4%be%86xcode%e7%9b%b8%e9%97%9c%e9%87%8d%e6%96%b0%e8%a8%ad%e5%ae%9a
title: '[iOS][XCODE] 重灌Mac Air所帶來xcode相關重新設定'
wordpress_id: 1275
categories:
- iOS的程式開發
---



  * SDK需要重新安裝與包裝，這次打算放到dropbox可能可以方便下次有富源需求(希望不要)


  * iOS Development Certificate的問題



    * 當初我有保留以下檔案



      * AppleWWDRCA.cer


      * CertificateSigningRequest.certSigningRequest


      * Evan_Lin_iPhone4.mobileprovision


      * ios_development.cer



    * 不過還是沒辦法，因為private key 不見了，所以還是得重新申請，參考以下方法



      *  Connect to the apple developer member center then the iOS provisional portal.




      * Revoke my certificate.


      * Create a new certificate by providing a new pair of private and public key.


      * Remove all the previous provisioning profiles and create new ones.


      * Download the new provisioning profiles and install them in XCode by just dragging them to the XCode icon in the dock.


      * refer 



        * [http://stackoverflow.com/questions/6769345/xcode-4-valid-signing-identity-not-found-error-on-provisioning-profiles-on-a](http://stackoverflow.com/questions/6769345/xcode-4-valid-signing-identity-not-found-error-on-provisioning-profiles-on-a)


        * [http://adalin05.pixnet.net/blog/post/26454479-iphone%E9%96%8B%E7%99%BC%E7%AD%86%E8%A8%98%EF%BC%9Aerror-message](http://adalin05.pixnet.net/blog/post/26454479-iphone%E9%96%8B%E7%99%BC%E7%AD%86%E8%A8%98%EF%BC%9Aerror-message)





  * 遇到"A valid provisioning profile matching the application's Identifier could not be found"



    * 雖然certificate都安裝好了～還是有可能遇到問題


    * 這時候解法是 xcode -> windows -> Orgnizer 然後 editor->refresh from developer portal 



      * refer: [http://forum.geego.com/forums/modules/newbb/viewtopic.php?topic_id=623&forum=17](http://forum.geego.com/forums/modules/newbb/viewtopic.php?topic_id=623&forum=17)



    * 之後要到xcode設定把 target 從iOS device 改道 iPhone



      * refer:  [http://stackoverflow.com/questions/16155613/a-valid-provisioning-profile-matching-the-applications-identifier-could-not-be](http://stackoverflow.com/questions/16155613/a-valid-provisioning-profile-matching-the-applications-identifier-could-not-be)




  * 重開xcode 或是重新 clean -> build 就可以


