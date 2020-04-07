---
layout: post
layout: post
title: "[iOS][XCode][AutoBuild]建立XCode Build Script 來自動產生連接好的多平台Libraries"
description: ""
category: 
- ios的程式開發
tags: []

---


## 主要問題:

在iOS上面要編譯 C/C++ Static Library，除了有必要的轉換之外．最麻煩的大概就是先把多個平台`iphoneos`跟`iphonesimulator`的產出library透過`lipo` 結合成一個所謂的 **fat library**之後，在複製到某個地方．

但是在Windows VC開發已經習慣使用Post Build Process的我，當然在XCode開發上面也要找出一套自動的方式來幫我完成這件很蠢的事情．


### 解決流程:


#### 建立一個Aggregate

透過編輯器 [Editor] -> [Add Target] 來建立一個Aggregate (記得之後要build就要選擇這個target)． 至於選擇Mac 或是 iOS的沒有差異．  這邊只需要輸入一個target 名稱後就可以了．


##### 建立一個 Run Script

![image](https://developer.apple.com/library/ios/recipes/xcode_help-project_editor/Art/run_script_configuration_2x.png)
(圖片來自 Apple Developer)

如同上圖所示，在Target 選擇設定，選擇剛剛建立的Aggregate，然後到 [Build Phase]裡面去 點擊`+` 來新增 "Run Script"

以下為範例的Build Script，主要是把專案"ABC"的Debug-iphoneos 跟 Debug-iphonesimulator 的輸出結合到另外一個目錄．

        xcodebuild -project abc.xcodeproj -scheme ABC -sdk iphonesimulator -configuration Debug
        xcodebuild -project abc.xcodeproj -scheme ABC -sdk iphoneos -configuration Debug
        
        # make a new output folder
        mkdir -p ${TARGET_BUILD_DIR}/../outputABC
        
        # combine lib files for various platforms into one
        lipo -create "${TARGET_BUILD_DIR}/../Debug-iphoneos/libABC.a" "${TARGET_BUILD_DIR}/../Debug-iphonesimulator/libABC.a" -output "${TARGET_BUILD_DIR}/../outputCCS/libABC.a"


當然，你也可以把檔案輸出到專案目錄下面，方便你以後管理．

#### 衍生應用

你可以每次build的時候增加版本號碼，這是其他人比較喜歡用的． 可以參考[這篇](http://nelson.logdown.com/posts/2014/01/27/easy-version-control-mechanism-for-xcode-projects)

### 心得與應用:

這一篇文章，主要就是讓Engineer可以簡單地透過`Cmd + b`來編譯一個特殊的Aggregate target來執行一堆本來是透過CI來跑的 Script． 這樣一來，Engineer可以邊修改，邊把相關檔案編譯出來．  而最後要導入CI的時候，該Aggregate target也可以直接拿來編譯．


## 參考文章:

- [Automatic build of static library for iOS and many architectures](http://blog.sigmapoint.pl/automatic-build-of-static-library-for-ios-for-many-architectures/)
- [Apple Developer:Running a Script While Building a Product](https://developer.apple.com/library/ios/recipes/xcode_help-project_editor/Articles/AddingaRunScriptBuildPhase.html)
- [更複雜的iOS build tool: Thoughts on iOS build tools](https://krausefx.com/blog/ios-tools?utm_campaign=CodeBaku&utm_medium=email&utm_source=CodeBaku_4)
