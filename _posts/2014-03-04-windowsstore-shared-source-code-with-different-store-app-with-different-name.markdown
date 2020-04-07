---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-04 07:47:46+00:00
slug: windowsstore-%e5%85%a9%e5%80%8b%e7%94%a2%e5%93%81%e5%85%b1%e7%94%a8%e5%90%8c%e4%b8%80%e4%bb%bd%e7%a8%8b%e5%bc%8f%e7%a2%bc-shared-source-code-with-different-store-app-with-different-name
title: '[WindowsStore] 兩個產品共用同一份程式碼 (Shared source code with different store app with
  different name)'
wordpress_id: 1342
categories:
- C#相關學習
- 學習文件
- 下一代視窗系統心得
---

**起因:**  
主要是因為在Windows Store 上面，需要產生兩個相同的內容的產品  
但是版號與產品名稱卻不能相同．




**做法：**






  * 原本做法



    * 複製一個相同的目錄 （原本稱為ㄓ ProjA 複製出來為 ProjB)


    * 修改 csproj 把每個檔案的鏈結改到原本的 檔案


    * 在搬移的過程中，會出現以下的錯誤



      * Could not find **.xbf in target folder



    * 查詢過後發現，XAML 的檔案無法去link具有上一層目錄的檔案架構 ( ....XXX)



      * 但是卻可以去link "XXX “




  * 解決方法:



    * 僅僅複製把ProjB 的需要的檔案到ProjA的目錄下



      * AssemblyInfo.cs (注意要改名)


      * XXX.csproj


      * XXX_StoreKey.pfx


      * XXX_TemporaryKey.pfx


      * Package_StoreAssociation.xml (注意要改名)


      * Package.appxmanifest(注意要改名)



    * 這樣下來是有點醜，因為 bin/obj 會共用～這個之後會再仔細觀察是否有任何問題


    * 此外需要改兩個東西



      * <ProjectID> 在 csproj 裡面


      * <Identifier Name> 在 Package.appxmanifest與Package_StoreAssociation.xml 內



    * 這樣就可以產生兩個一樣內容的app 在你的桌面





**參考:**




[http://stackoverflow.com/questions/8162179/how-do-i-install-two-versions-of-my-metro-app](http://stackoverflow.com/questions/8162179/how-do-i-install-two-versions-of-my-metro-app)
