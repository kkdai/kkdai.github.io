---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-12-11 18:23:29+00:00
slug: '%e5%a5%bd%e7%94%a8%e5%b0%8f%e5%b7%a5%e5%85%b7-%e6%9b%bf%e6%8f%9bmcewmp-%e7%9a%84-video-decoder-%e5%bd%b1%e5%83%8f%e8%a7%a3%e7%a2%bc%e5%99%a8'
title: 好用小工具–替換MCE/WMP 的 Video decoder (影像解碼器)
wordpress_id: 1001
categories:
- 網路上好玩的事情
- 下一代視窗系統心得
---

 

Vista MCE(Meida Center)是一個相當強大而好用的全螢幕播放程式(也跟XBOX360裡面一樣)。他雖然好用～ 但是MS的影像解碼器一樣只是夠用就好，如果想要讓你的MCE效率更好、畫面更漂亮。通常的選擇是去換一套撥放軟體(WINDVD或是POWERDVD)但是~~~ Vista MediaCenter Decoder Utility是一個相當好用的小軟體，可以讓你很快速的替換掉他的VIDEO Decoder(影像解碼器)。 這樣一來你就不需要換軟體就可以有更高的效能與支援。

 

[![clip_image002](http://www.evanlin.com/wp/wp-content/uploads/2008/12/clip-image002-thumb.gif)](http://www.evanlin.com/wp/wp-content/uploads/2008/12/clip-image002.gif)

 

Download [http://www.whittakermoore.com/VMCD.exe](http://www.whittakermoore.com/VMCD.exe)

 

Detail description: [http://tdnj.pixnet.net/blog/post/13748926](http://tdnj.pixnet.net/blog/post/13748926)

 

 

 

[![WMP_Dec.jpg](http://farm4.static.flickr.com/3142/3100170548_9e0a1c0dd1.jpg)](http://www.flickr.com/photos/27643002@N00/3100170548/)

 

**《說明》如何讓 Microsoft Media Player 使用不同的 MPEG Decoder        
(資料來源) [http://www.zhe-feng.com.tw/forum/archiver/?tid-601.html](http://www.zhe-feng.com.tw/forum/archiver/?tid-601.html) **

 

一般我們在使用 Media Player 時, 它會自動去抓取系統中可用的 MPEG Decoder, 雖然這樣很簡單又方便, 但是遇到這個 Decoder 表現不好, 例如去交錯做不好或者沒有做, 或者 CPU 使用率太高, 亦或畫質表現不佳等等. 我們可以讓它去選取另一個在系統中可用的 MPEG Decoder 嗎? 當然可以, 本文就是解說要如何讓它改用你指定的 Decoder. ( ** MPEG CODEC 請自行安裝 ** )      
首先, 我們要先下載一個工具軟體       
Windows XP Video Decoder Checkup Utility (在 VISTA 下也可以用來設定 Media Player)       
[http://www.microsoft.com/downloads/details.aspx?FamilyID=de1491ac-0ab6-4990-943d-627e6ade9fcb&displaylang=en&Hash=zB70IgOupjB7du1zYHSvaIUsfUP4rxLtY4Kga%2fFcR%2fVtbbQyzHsjDX2dnP7AOtWyMRhEpPsT5mAo%2bHFS8gXi%2fA%3d%3d]http://www.microsoft.com/downloads/details.aspx?FamilyID=de1491ac-0ab6-4990-943d-627e6ade9fcb&displaylang=en&Hash=zB70IgOupjB7du1zYHSvaIUsfUP4rxLtY4Kga%2fFcR%2fVtbbQyzHsjDX2dnP7AOtWyMRhEpPsT5mAo%2bHFS8gXi%2fA%3d%3d](http://www.microsoft.com/downloads/details.aspx?FamilyID=de1491ac-0ab6-4990-943d-627e6ade9fcb&displaylang=en&Hash=zB70IgOupjB7du1zYHSvaIUsfUP4rxLtY4Kga%2fFcR%2fVtbbQyzHsjDX2dnP7AOtWyMRhEpPsT5mAo%2bHFS8gXi%2fA%3d%3d%5dhttp://www.microsoft.com/downloads/details.aspx?FamilyID=de1491ac-0ab6-4990-943d-627e6ade9fcb&displaylang=en&Hash=zB70IgOupjB7du1zYHSvaIUsfUP4rxLtY4Kga%2fFcR%2fVtbbQyzHsjDX2dnP7AOtWyMRhEpPsT5mAo%2bHFS8gXi%2fA%3d%3d)

 

我們將會下載到一個 DECCHECKSetup.EXE, 執行它後會開始安裝, 過程很簡單, 所以不再詳述.      
安裝完畢後, 在安裝的路徑下會看到一個執行檔 DECCHECK.exe, 它就是本篇文章的主角. 在開始執行它之前, 請確定你的 XP 系統登入權限是 Administrator, 或這等級的 Group. 不然會出現下列訊號       
[attach]961[/attach]       
在 VISTA 系統下, 除了在 Administrator 權限, 仍要加入以下操作 :       
請將滑鼠移到 DECCHECK.exe 執行檔位置, 按滑鼠右鍵會出現下圖.
