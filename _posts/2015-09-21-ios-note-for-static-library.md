---
layout: post
layout: post
title: "[iOS]使用iOS Xcode編譯static library一些筆記"
description: ""
category: 
- ios的程式開發
tags: []
---


![image](http://a2.mzstatic.com/us/r30/Purple3/v4/a3/6e/90/a36e9037-8a0d-e142-c0d8-40ecb23abf45/icon128-2x.png)

####將所有m透過預設Object C++來編譯:
     
- 設定->LLVM 7.0 -> Compile Source As -> `Object-C++`

這裏的設定比起單獨點選每個.m再來改成ObjectC++更有優先權．


##### 設定額外的Include Path

- 設定-> HEADER_SEARCH_PATHS (記得改HEADER_SEARCH_PATHS就好)．
- 記得要使用 `$(PROJECT_DIR)/../../../include`，而不是單純的 `../../../include`不然會找不到．

#### 避免發生 `Undefined symbols for architecture x86_64`

這是因為你嘗試要跑模擬器的編譯而你編譯出來的static library並沒有x86_64的選項．這邊的修改方式有幾個:

- 增加`x86_64` 到"Build Setting"->"Artchitectures"跟"Valid Artchitectures"
- 或是修改"Build Setting"->"Build Active Architecture Only"改成`No`


#### Bitcode enableing

[Bitcode](https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Conceptual/WhatsNewXcode/Articles/xcode_7_0.html)是xcode7新的功能．但是如果使用xcode新的專案都會遇到warning，關閉的方法就是在App去關閉，當然如果要使用這個功能，就得每個library都要打開bitcode．

