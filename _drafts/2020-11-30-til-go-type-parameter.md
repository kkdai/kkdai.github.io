---
layout: post
title: "[TIL][Golang]  Type Parameter in Golang 初體驗"
description: ""
category: 
- TodayILearn
tags: ["Golang", "OpenSource"]
---



## 前言:

Generics (泛型)一直是 Golang 這個程式語言比較受到 C++ 與 Java 轉過來的開發者們經常訓問的問題。 這邊幫大家整理一下與嘗試一下最新版本的 Go2 到底 Generics 狀況如何了。



## 為何程式語言需要 Generics
根據文章 "[Why Gnerics](https://blog.golang.org/why-generics)" 曾經舉過這個很棒的範例。先假設你需要將一個 slice 裡面所有元素從小到大來排序。

根據 Int 你可能會寫:

```Golang
func ReverseInts(s []int) {
    first := 0
    last := len(s) - 1
    for first < last {
        s[first], s[last] = s[last], s[first]
        first++
        last--
    }
}
```

而如果是字串的時候，可能如下：

```
func ReverseInts(s []int) {
    first := 0
    last := len(s) - 1
    for first < last {
        s[first], s[last] = s[last], s[first]
        first++
        last--
    }
}
```

根據以上的方式，你會發現兩個 function 其實真的沒有任何的差異。但是卻由於資料格式不同，需要特地用兩個 function 分開來撰寫。 這樣對於維護上往往不直覺，並且

## Golang 真的沒有其他替代方案嗎？


## 介紹影片:

<iframe width="560" height="315" src="https://www.youtube.com/embed/WzgLqE-3IhY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



## 相關文章：

- Golang Blog: 2019/07/31 "[Why Gnerics](https://blog.golang.org/why-generics)"
- Golang Blog: https://blog.golang.org/generics-next-step
- https://go.googlesource.com/proposal/+/refs/heads/master/design/go2draft-type-parameters.md
- https://go.googlesource.com/proposal/+/master/design/15292/2013-12-type-params.md
- https://groups.google.com/g/golang-dev/c/U7eW9i0cqmo/m/-gDfa_6KAAAJ?fbclid=IwAR27mCQ8vgV9w8A201SlLMkyTnWJbfKVBoVRFutGU1zt1_KOCib9pVeQSMs
- 
- Go2 Playground https://go2goplay.golang.org/p/-L6Zv2SOiAP?fbclid=IwAR29b5ECb1pK6YfmgGJROFoN3-NFovUmRrECjsr8LdRkwUGADfAVMP2tMI8
- 