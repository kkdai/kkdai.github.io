---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-13 01:54:28+00:00
slug: ios-%e4%b8%8a%e6%9e%b6app%e5%88%b0apple%e5%8e%bbreview%e5%89%8d%e6%9c%83%e9%81%87%e5%88%b0%e7%9a%84%e4%b8%80%e4%ba%9b%e5%95%8f%e9%a1%8c-%e4%bd%bf%e7%94%a8-xcode-organizer-%e4%be%86%e9%81%9e
title: '[iOS] 上架App到Apple去review前會遇到的一些問題-- 使用 XCode Organizer 來遞交App到Store'
wordpress_id: 1350
categories:
- iOS的程式開發
- 學習文件
---

聽從一些前輩的建議，決定還是把已經寫到一半卻累到半死的iOS App來上架  
這裡記錄一下上架之前會遇到的一些問題: 




首先要先連到 iTune Connect 去新增一個App(當然你需要開發者帳號，也就是付每年$99的費用)  
詳細流程參考: [http://www.minwt.com/ios/4726.html](http://www.minwt.com/ios/4726.html) 




關於新增資料上面有一些翻譯可以參考: [http://www.csdtn.net/article/2011-01-07/289703](http://www.csdn.net/article/2011-01-07/289703)




資料都新增完了，就會使用Application Loader 去上傳你寫好的App，不過這裡我發生幾次錯誤，分享給大家．






  *  _NSSetlogCstringFunction error on Application loader



    * 一開始我以為是NSLog的原因，不過主要的原因是因為跑到debug版本


    * 記得到 [Product] —> [Edit Schema] —> Change run to release．


    * 正確的包裝app方法為:



      * device —> iOS device


      * [Product] —> [Archive]




  * Provisioning missing 


  * iOS Apps must contain a provisioning profile in a file named embedded.mobileprovision.


  * Provisioning bundle identifier not match



    * 這三個問題出現的原因其實都一樣，一般而言讓App上架的方式有兩種:



      * 透過Application Loading，然後自己把App包裝起來以後上傳到iTune Connect


      * 透過Xcode Organizer 來做validate 與 上傳...  這也是我推薦的方式，因為比較容易看清楚所有的問題所在．


      * 接下來我會把我上傳的流程寫清楚(原諒都是文字，放圖實在有點懶)



    * 參考：



      * [http://stackoverflow.com/questions/5313537/bundle-identifier-and-provisioning-profile](http://stackoverflow.com/questions/5313537/bundle-identifier-and-provisioning-profile)


      * [http://lamb-mei.com/10/ios-ios-provisioning-portal-certificates/](http://lamb-mei.com/10/ios-ios-provisioning-portal-certificates/)


      * [http://kenobiluh.blogspot.tw/2012/02/certificate-provisioning-file-code.html](http://kenobiluh.blogspot.tw/2012/02/certificate-provisioning-file-code.html)






**使用 XCode Organizer 來遞交App到Store**




詳細方式如下：






  * 先到 Xcode 裏面Project Property —> [General] —> 把 Bundle Identifier 抄下來．記得把[team] 也先選擇到你的開發者帳號



    * ex: com.XXX.youAppName



  * 到[ iTune Connect](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CC0QFjAA&url=https%3A%2F%2Fitunesconnect.apple.com%2F&ei=WQ8hU_HsFY-ElAWI8oHABQ&usg=AFQjCNFepkqe89lSXO97jEa4MMdkHfX0RQ&sig2=XAW1MYEr3g8JyBPnZH7Gig) 新增App申請，詳細流程可以參考[這裡](http://www.minwt.com/ios/4726.html)．注意 Bundle ID要跟XCode裡面的相同(就算寫錯了～可以之後修改)


  * 到[Apple iOS Developer](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CC0QFjAA&url=https%3A%2F%2Fdeveloper.apple.com%2Fdevcenter%2Fios%2Findex.action&ei=QQ0hU8bOJMS3kAXazoHwBQ&usg=AFQjCNHxDaTP5oikxWseNxH-LYQjY4P28g&sig2=6PxJWQZV_XJp5CqvXeEf5g) 網站的相關處理:



    * [Certificates, Identifiers & Profiles] —>[Identifiers]—> 新增一個 iOS App ID



      * 記得這裡Bundle Identifier 要跟你Xcode設定裡面一樣



        * ex: com.XXX.youAppName


        * 



    * [Certificates, Identifiers & Profiles] —> [Provisioning Profiles] —> 新增一個[Distribution]的 Provisioning



      * 這裡的App ID要使用剛剛申請的，這裡最好是一對一的mapping 比較不會有問題．



    * 下載 Provisioning 並且點兩下安裝



  * 到Xcode 準備打包上傳



    * 先到Project Property —>[Build Setting] 搜尋 [PROVISIONING PROFILE] —> 將它改成你剛剛下載的Provisioning


    * 把device 從模擬器或是手機切換到 [iOS Device]


    * [Product] —> [Archive]



      * 如還是有出現問題請參照以上得問題解答



    * 這時候就可以做App Validation 跟 Distribute 





大致流程就是這樣～接下來我的App就等著Apple 審核～～也祝福大家都成功啦.... 
