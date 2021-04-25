---
layout: post
title: "[學習心得][Android] Android Study Jam 2021 - 心得"
description: ""
category: 
- 學習文件
tags: ["Android", "Kotlin"]
---

<img src="https://lh3.googleusercontent.com/c8Xn2bmNXtrRxy1DTwpkZrkeMxsZ3097rnTGqRBRIlQKL18_8G3-3UdZK3R8_gShtmXjRoceXiOlLUZh_BHNFcwc_BkVB2bDNYA=w2400-h2111-c" width="400px">



## 前言:








## TL;DR 

本篇文將要介紹以下一些的部分：

- <a href="#setup">如何將舊的開源專案支援 Go Modules </a>
- <a href="#summary">結論</a>
- <a href="#refer">參考文章</a>
  


## 如何設定環境(How to setup your Android 4.1 on Mac Catalina)

<a id="setup"></a>

跟著這次的 Kotlin coroutine codelab [你會找到 Android Studio 下載的地方](https://developer.android.com/studio/)。 你也會找到相關的 [github for Kotlin coroutine](https://github.com/googlecodelabs/kotlin-coroutines) 。 然後透過 Android Studio 來打開 `coroutines-codelab` 的資料夾，你可能會出現以下的 Error 。

> License for package Android SDK Build-Tools 29.0.2 not accepted
>
> License for package Android SDK Platform 29 not accepted.

[Preference] -> [System Setting] -> [Android SDK] -> Enable Android Platform 29

這樣下載 Android SDK 29 的時候，就會同時去同意 License 。



> Could not resolve all dependencies for configuration ':start:debugRuntimeClasspath'.
> Could not create task ':start:minifyReleaseWithR8'.
> Cannot query the value of this provider because it has no value available.



## 結論：

<a id="summary"></a>





## 相關文章：
<a id="refer"></a>

- [Stackoverflow: You have not accepted the license agreements of the following SDK components duplicate](https://stackoverflow.com/questions/39760172/you-have-not-accepted-the-license-agreements-of-the-following-sdk-components)

