---
layout: post
title: "[TIL][Kotlin] 關於 Kotlin 相關語法快速整理"
description: ""
category: 
- 學習文件
tags: ["Android", "Kotlin"]
---

<img src="https://logo-logos.com/wp-content/uploads/2016/10/Kotlin_logo_image_picture.png" width="400px">



## 前言:

最近 Google Taiwan 開啟一個計畫「[Android App 開發培訓計劃 2021](https://events.withgoogle.com/android-study-jam-twhk-2021/)」，透過 Android Studio 跟著 Kotlin 這個語言來讓大家寫 Android App 。趁機順便學習一下 Kotilin 相關語法跟需要注意的地方。




## 相關工具 

<a id="setup"></a>

#### Kotlin Playground

由於有點懶得安裝環境跟相關的 IDE 設定，所有的練習其實可以快速的在 [Kotlin Playground](https://play.kotlinlang.org/) 上面完成。

#### 語法Tutorial - Koans

可以透過網頁上面的 [Koans](https://play.kotlinlang.org/koans) 或是內建在 JetBrains [IntelliJ IDEA or Android Studio 的 JetBrains educational plugin](https://www.jetbrains.com/help/education/learner-start-guide.html?section=Kotlin Koans) 



## 學習建議：

由於這個語言我不是很熟習，但是熟悉新的語法其實蠻建議透過 Tutorial 來先有個概念性的了解，方便你開始相關的查詢。 所以這邊建議先跑過一次  [Koans](https://play.kotlinlang.org/koans)  再來把相關資料結構與語法來做一個語法的相關脈絡整理。



## 特別資料結構語法說明

### LIST 

```
val numbers: List<Int> = listOf(1, 2, 3, 4, 5, 6)
```

透過 `List` 關鍵字來加上 generic type 來建立出相關 List 物件。也可以透過 Inferred 方式建立。

```
val numbers = listOf(1, 2, 3, 4, 5, 6)
```

Kotlin 是 Zero-based Index 的語言（延伸自 Java) 。

### MutableList

參考[官方文件](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-mutable-list/) ， MutableList 為長度可變的 List 。也就是可以針對 List 去 add, addAll, listIterator, remove, removeAll... 相關操作。 宣告方式參考如下：

```
val entrees = mutableListOf<String>()
```





## 相關文章：

<a id="refer"></a>

- [Stackoverflow: You have not accepted the license agreements of the following SDK components duplicate](https://stackoverflow.com/questions/39760172/you-have-not-accepted-the-license-agreements-of-the-following-sdk-components)

