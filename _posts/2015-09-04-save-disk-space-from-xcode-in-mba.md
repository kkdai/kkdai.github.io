---
layout: post
layout: post
title: "[MBA]砍掉XCode的iPhone debug symbol 來節省MacBook Air的小SSD"
description: ""
category: 
- 柴米油鹽的東西
tags: []

---

##前言:

當初錯誤的決定，MBA 128GB 造就我很愛花時間找有沒有東西可以清， 分享一下我使用Mac Book Air 128G 尋找空間的心得．

## 查看每個程式佔多大空間


### 顯示隱藏資料夾

這邊需要console指令，方法為以下:

- 開啟一個showHiddenFiles.sh `vi showHiddenFiles.sh`
- 輸入以下指令:


        defaults write com.apple.finder AppleShowAllFiles TRUE
        killall Finder


- 存擋，並且記得把權限打開`chmod 660 showHiddenFiles.sh`
- 執行這個會把所有finder都關閉，但是可以之後可以顯示所有隱藏檔案  ( .git 也會出現)


### 恢復隱藏資料夾

反之，如果要把隱藏資料再次的隱藏起來，可以輸入以下的指令:

        defaults write com.apple.finder AppleShowAllFiles FALSE
        killall Finder

### 讓Finder裡面顯示資料夾空間

![image](http://www.iclarified.com/images/tutorials/14356/49356/49356.png)

(圖片來自: [http://www.iclarified.com/14356/how-to-display-folder-size-in-mac-os-x-finder](http://www.iclarified.com/14356/how-to-display-folder-size-in-mac-os-x-finder))

主要的方式是到[Finder Menu]->[顯示方式: View]->[打開顯示方式選項: Show View Options]->[計算所有大小]


### 砍掉Xcode Debug Symbol

主要是參考[這篇文章](http://blog.favo.org/post/31649090293/xcode-5-places-to-save-some-disk-space) 提到的第四項可以清掉一些xcode debug symbol．每個OS版本也接近1G. 這樣清下去也有20~30G 空間
就算多砍也沒差，反正你手機接起來要debug又會把他都複製到電腦裡面了．

- 到`~/Library/Developer/Xcode/iOS DeviceSupport/`
- 裡面會顯示許多版本的iPhone Debug Symbol (ex: `8.3 (12F70)`... )
- 移除掉比較舊的版本Debug Symbol

跟我一樣MBA SSD快爆的人來看看... 馬上又一條活龍...
