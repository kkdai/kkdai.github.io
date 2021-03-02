---
layout: post
title: "[學習心得][Golang] Go 1.16 之後的版本撤回方式 Go Modules retraction"
description: ""
category: 
- TodayILearn
- 學習文件
tags: ["Golang", "DevOps", "CICD"]
---



![](https://i.ytimg.com/vi/23JqUVHV7_Q/maxresdefault.jpg)

## 前言:

Go Module 在 1.11 的版本正式導入了 [Golang Modules](https://blog.golang.org/using-go-modules) 讓許多套件可以使用 Go Module 來管理相依 (Dependency) 的套件。並且在 Go 1.16 版本也預設開啟了 Go Modules 的選項。但是在開發套件 (Package) 的時候可能會發生以下的問題：

- 忽然發現某個的套件有重大的問題，希望其他人不要使用到這個套件。
- 不小心進版號進太多了，而且有一些人也使用到這些版本。 (e.g 本來要跑 v0.4.0 ，結果不小心寫成 v1.0.0 )

以上兩個問題，如果在套件還沒有散佈出去的話，其實都是沒有問題的。但是如果套件也散布出去的話，就需要透過套件的撤回（Retract) ，來讓使用套件的開發者能了解相關的問題，也讓之後使用的人不會再用到這個版號。

本篇文章將會介紹 Go 1.16 裡面一個比較沒有被重點宣傳的功能（大部分人注意的是 Apple M1 支援），並且透過官方給的線上範例也給版本撤回的實作。

## TL;DR 

本篇文將要介紹：

- <a href="#what-is-retraction">什麼是 Retraction</a>

- <a href="#why-retraction">為什麼需要 Retraction</a>

- <a href="#old-way-retraction">以往要如合作撤回版本的方式</a>

- <a href="#howto-retraction">如何使用 Go modules Retraction </a>

- <a href="#retraction-reference">相關學習資源</a>




## 什麼是 Retraction ?

<a id="what-is-retraction"></a>

顧名思義就是版本的撤回，也就是將「有問題」的版本將以撤回。



## 為何需要 Retraction ?

<a id="why-retraction"></a>

通常有兩類的問題



## 以往要如合作撤回版本的方式

<a id="old-way-retraction"></a>



## 如何使用 Go modules Retraction 

<a id="howto-retraction"></a>



## 相關學習資源

<a id="retraction-reference"></a>



## 相關文章：

- [Using Go Modules](https://blog.golang.org/using-go-modules)
- [Go Dev Playground: Retract Module Versions](https://play-with-go.dev/retract-module-versions_go116_en/)
- [Retract Go Module Versions in Go 1.16](https://golangtutorial.dev/tips/retract-go-module-versions/)
