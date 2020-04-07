---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-02-19 16:36:44+00:00
slug: metroappc-%e5%88%a9%e7%94%a8%e7%a8%8b%e5%bc%8f%e4%be%86%e5%8b%95%e6%85%8b%e7%9a%84%e4%bf%ae%e6%94%b9%e5%9c%96%e7%89%87-change-metrostore-app-image-source-via-code-2
title: '[MetroApp][C#] 利用程式來動態的修改圖片 (Change Metro/Store App image source via code)'
wordpress_id: 1329
categories:
- C#相關學習
- NET程式設計
- 下一代視窗系統心得
---

其實應該是不難，只是一直很難找到正確的解答．可能是WPF，Windows Phone與Store App會有不同的結果，稍微記錄一下．




**需求：**  
在Windows Store App中，有些圖片，希望可以用程式的方式來決定讀取的方式．




**解法：**




找了半天，不論是



    
    <code style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;font-family:Consolas, Menlo, Monaco, 'Lucida Console', 'Liberation Mono', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Courier New', monospace, serif;"><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pln">img1</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">.</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;background-position:initial initial;background-repeat:initial initial;" class="typ">Source</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">=</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#00008b;background-position:initial initial;background-repeat:initial initial;" class="kwd">new</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;background-position:initial initial;background-repeat:initial initial;" class="typ">BitmapImage</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">(</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#00008b;background-position:initial initial;background-repeat:initial initial;" class="kwd">new</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;background-position:initial initial;background-repeat:initial initial;" class="typ">Uri</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">(</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#800000;background-position:initial initial;background-repeat:initial initial;" class="str">"ms-appx:///Assets/Common_Pic/image2.png"</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">,</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;background-position:initial initial;background-repeat:initial initial;" class="typ">UriKi</span></code>




甚至也把URI 換成 System.Uri也是不行．




後來找到這篇文章才終於搞定[Load Image Dynamically From Application Uri in Windows Store Apps](http://www.c-sharpcorner.com/UploadFile/99bb20/load-image-dynamically-from-application-uri-in-windows-store/)




步驟寫一下:






  * 首先千萬記得把你要變動的圖片記得讀進project 中，內容可以設定為 content. 檔案方式可以用成copy if newer


  * 之前主要失敗的原因是，檔案在那個時候其實還沒有完全讀完畢，所以需要用 StorageFile 來讀取


  * 記得讀取檔案是 async 的動作，所以得用 await 強制它讀完繼續執行


  * 最後再把檔案用BitmpImage包起來並且傳給 image.Source




完整程式碼可參考底下～或是去看鏈結





 




**參考:**






  * 主要解法: [http://www.c-sharpcorner.com/UploadFile/99bb20/load-image-dynamically-from-application-uri-in-windows-store/](http://www.c-sharpcorner.com/UploadFile/99bb20/load-image-dynamically-from-application-uri-in-windows-store/)


  * 其他問相關問題～當做參考吧



    * [http://stackoverflow.com/questions/21283400/how-to-change-images-source-windows-store-app-with-code/21881311#21881311](http://stackoverflow.com/questions/21283400/how-to-change-images-source-windows-store-app-with-code/21881311#21881311)


    * [http://stackoverflow.com/questions/12450906/metro-windows-store-app-relative-image-source-binding-does-not-work](http://stackoverflow.com/questions/12450906/metro-windows-store-app-relative-image-source-binding-does-not-work)


    * [http://stackoverflow.com/questions/11836032/setting-image-source-programatically-in-metro-app-image-doesnt-appear/21881098#21881098](http://stackoverflow.com/questions/11836032/setting-image-source-programatically-in-metro-app-image-doesnt-appear/21881098#21881098)



