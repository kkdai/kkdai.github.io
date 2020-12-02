---
layout: post
title: "[TIL][Golang]  Type Parameters in Golang 初體驗 (a.k.a Go2 Generics)"
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
func ReverseInts(s []int) []int {
    first := 0
    last := len(s) - 1
    for first < last {
        s[first], s[last] = s[last], s[first]
        first++
        last--
    }
    return s
}
```

而如果是字串的時候，可能如下：

```
func ReverseInts(s []string) []sting {
    first := 0
    last := len(s) - 1
    for first < last {
        s[first], s[last] = s[last], s[first]
        first++
        last--
    }
    return s
}
```

根據以上的方式，你會發現兩個 function 其實真的沒有任何的差異。但是卻由於資料格式不同，需要特地用兩個 function 分開來撰寫。 這樣對於維護上往往不直覺，並且

## Golang 真的沒有其他替代方案嗎？

先不談 Generics ，其實 Golang 可以透過 Interfaces 的方式來做相關的開發，這裡是相關的



## Golang Generic Proposal 介紹影片:



<iframe width="560" height="315" src="https://www.youtube.com/embed/WzgLqE-3IhY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



## 試玩 Go2 Playground

就讓我們來透過 Go2 playground 提供的 Type Parameters 來回頭看我們原本的問題。是否有方式可以透過 `Type Parameters` 來實作呢？

馬上來看結果: https://go2goplay.golang.org/p/doitUP4_1Jm

```
func ReverseSlice[T any](s []T) []T {
	first := 0
	last := len(s) - 1
	for first < last {
		s[first], s[last] = s[last], s[first]
		first++
		last--
	}
	return s
}

func main() {
	fmt.Println(ReverseSlice([]string{"Hello", "playground"}))
	fmt.Println(ReverseSlice([]int{1, 3, 5}))
}

```



## Type Parameters加上限制

使用 `any` Type Parameter 其實相當的方便，但是往往取決於你可能處理的資料並不適合所有的型態的時候。其實需要加上一些資料的限制。 舉個例子來說明：

#### 範例： 兩個參數取比較大的

```
	if a < b {
		return a
	}
	return b
```

這是一個相當簡單的比較方式，但是可以看到如果將這個方式透過 Type Parameters 來撰寫。會發現輸入的參數將不支援 string 



## 相關文章：

- Golang Blog: 2019/07/31 "[Why Gnerics](https://blog.golang.org/why-generics)"

- Golang Blog: https://blog.golang.org/generics-next-step

- Type Parameters in Go [https://go.googlesource.com/proposal/+/master/design/15292/2013-12-type-params.md](https://go.googlesource.com/proposal/+/master/design/15292/2013-12-type-params.md)

- https://groups.google.com/g/golang-dev/c/U7eW9i0cqmo/m/-gDfa_6KAAAJ?fbclid=IwAR27mCQ8vgV9w8A201SlLMkyTnWJbfKVBoVRFutGU1zt1_KOCib9pVeQSMs

- Type Parameters Draft Design in Gohttps://go.googlesource.com/proposal/+/refs/heads/master/design/go2draft-type-parameters.md

- Go2 Playground https://go2goplay.golang.org/p/-L6Zv2SOiAP?fbclid=IwAR29b5ECb1pK6YfmgGJROFoN3-NFovUmRrECjsr8LdRkwUGADfAVMP2tMI8

- 