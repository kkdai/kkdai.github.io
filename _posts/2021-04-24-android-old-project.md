---
layout: post
title: "[TIL][Android]  Android Studio 開啟專案可能會遇到的問題"
description: ""
category: 
- 學習文件
tags: ["Android", "Kotlin"]
---

<img src="https://lh3.googleusercontent.com/c8Xn2bmNXtrRxy1DTwpkZrkeMxsZ3097rnTGqRBRIlQKL18_8G3-3UdZK3R8_gShtmXjRoceXiOlLUZh_BHNFcwc_BkVB2bDNYA=w2400-h2111-c" width="400px">



## 前言:

最近 Google Taiwan 開啟一個計畫「[Android App 開發培訓計劃 2021](https://events.withgoogle.com/android-study-jam-twhk-2021/)」，透過 Android Studio 跟著 Kotlin 這個語言來讓大家寫 Android App 。雖然我之前有寫過，但是也很久沒使用 Android Studio 了，來開啟專案看看吧。 不過一開始好像開啟錯誤的 codelabs www 不小心開到後面的 [Use Kotlin Coroutines in your Android App](https://developer.android.com/codelabs/kotlin-coroutines)，出現了一些問題，也順便解決一下。




## 如何設定環境(How to setup your Android 4.1 on Mac Catalina)

<a id="setup"></a>

跟著這次的 Kotlin coroutine codelab [你會找到 Android Studio 下載的地方](https://developer.android.com/studio/)。 你也會找到相關的 [github for Kotlin coroutine](https://github.com/googlecodelabs/kotlin-coroutines) 。 然後透過 Android Studio 來打開 `coroutines-codelab` 的資料夾，你可能會出現以下的 Error 。

```
> License for package Android SDK Build-Tools 29.0.2 not accepted
> License for package Android SDK Platform 29 not accepted.

> Could not resolve all dependencies for configuration ':start:debugRuntimeClasspath'.
> Could not create task ':start:minifyReleaseWithR8'.
> Cannot query the value of this provider because it has no value available.
```

#### 原因:

因為最新版本的 Android Studio 4.1.3 ，預設使用的 Android SDK 版本是 30 ，但是這次的 codelab 是使用舊版本的 29 來準備的。所以需要下載舊版本的 SDK 並且同意相關的 License 授權。



#### 解決方式:

`[Preference] -> [System Setting] -> [Android SDK] -> Enable Android Platform 29`

這樣下載 Android SDK 29 的時候，就會同時去同意 License 。 就可以正常的 Compile。 範例也可以正常執行在 Android Simulator 上才是。





## 相關文章：
<a id="refer"></a>

- [Stackoverflow: You have not accepted the license agreements of the following SDK components duplicate](https://stackoverflow.com/questions/39760172/you-have-not-accepted-the-license-agreements-of-the-following-sdk-components)

