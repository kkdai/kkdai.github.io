---
layout: post
layout: post
title: "[GTUG][GDG]#11 Google Tag Manager 介紹"
description: ""
category: 
- 研討會心得

tags: []

---

**心得:**

原本想說要休息一兩個禮拜專心趕Mooc的作業，只是剛好作業進度都超前加上許久不見的Marx也會去，順便去了解一下．

[「Google Tag Manager 」](https://www.google.com/tagmanager/)對我而言是完全新的東西，雖然我比較常用Google Adsense跟Google Analytics． 



**簡介GTM**

做電子商務的網站的人都了解，如何有效地分析使用者的動作跟流量一直是一個很大的問題．所以這時候會在網頁上面加上許多的JS．但是每次類似的修改都得要動到網頁本身，對於有CDN作為cache的商家而言，是一個很困擾的事情．

Google Tag Manager(GTM)本身就是一個異地的library，可以寫在原本的網站的裡面．幫助作為網站分析與電子商務廣告的部分．

原本你想要增加新的功能(比如說增加Google Analytics，或是增加新的追蹤)是需要去修改原本的程式碼．

但是使用了GTM之後，就再也不用去修改原本的網站部分．只需要去管理頁面增加一些標記，規則或是巨集．也不用擔心cache與CDN造成的delay．


**速記:**

- 加上GTM前注意:
    - 最後的"dataLayer"要移除掉，因為這個東西很常被其他JS使用．
- Checking Pixel
    - 利用 1 pixel 的圖片，來帶參數以達到廣告作用．（偵測流量)
- Google Tag Manager主要有三個可以加入的東西
    - 標記
        - 增加或是搭配其他的服務(Google Analytics)
    - 規則
        - 增加許多規則，可以是網頁參數，可以是JS參數．
    - 巨集
        - 可以使用Regular Expression 來處理更多事情．
- 標記的"點擊接聽 "
    - 可以監聽所有網頁上的點擊動作(button 甚至是其他...)
    - 可以利用console 去查看dataLayer網頁上的點擊動作．
- Google Tag Manager的優點
    - 加上了Google Tag Manager他會加上亂數去破壞cache，對於有    使用CDN的網站而言．這樣才能達到及時更新的效果．
    - 也俱有程式碼最佳化，與瀏覽器的相容性問題
    - 本身也具有版本控管，不過講者建議使用Git(把code自行複製出來再貼到git)
- Google Analytics的進階使用:
    - 原本使用IP，現在使用session 可以偵測跨網域的使用者      
    
**參考資料:**    

- GDG活動網頁
    - [http://gdg-taipei.kktix.cc/events/google-tag-manager-intro](http://gdg-taipei.kktix.cc/events/google-tag-manager-intro)
- GTM (Google Tag Manager 官方網頁)    
    - [http://www.google.com/tagmanager/](http://www.google.com/tagmanager/)
- GTM 簡單的教學
    - [http://analyticsdavis.blogspot.tw/2013/11/google-tag-manager.html](http://analyticsdavis.blogspot.tw/2013/11/google-tag-manager.html)
- GTM 教學與簡介
    - [http://9i543.com/10665/websys](http://9i543.com/10665/websys)          
