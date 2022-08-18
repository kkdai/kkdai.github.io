---
layout: post
title: "[學習心得][Golang] Go 1.16 新功能的「版本撤回(下架)」(Go Modules retraction)"
description: ""
category: 
- TodayILearn
- 學習文件
tags: ["Golang", "DevOps", "CICD", "Gotips"]
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

通常有兩類的問題:

- 「已經發佈」的版本中，有某個版本發現致命的錯誤需要撤回。
  - 其中「已經發佈」代表已經公開發佈在 github (或其他 repository) ，並且有人使用。
- 不小心將版本號碼打錯了，比如說 0.4.0 的版本，後來不小心打成 1.0.0 但是又被人拿去使用。



## 以前要如合作撤回版本的方式

<a id="old-way-retraction"></a>

由於以往並沒有提供 Go Module Retraction ，所以發生以上情形，只能在 README 上面註解。 並沒有方式在 `go get ` 同時獲得足夠的資訊。



## 如何使用 Go modules Retraction 

<a id="howto-retraction"></a>

這裡透過線上 [Go Dev Playground](https://play-with-go.dev/retract-module-versions_go116_en/)，直接一步步講解主要的問題解決方式。  詳細的程式碼，可以到[裡面去](https://play-with-go.dev/retract-module-versions_go116_en/)查看。



### 問題 1: 發現有某個版本出現了重大問題怎麼辦?

假設你管理套件 `gopher.live/ue0ddd4a99c02/proverb` ，目前已經發佈到了 `0.2.0` 的版本出去。但是發現你這個版本有重大的問題。需要把這個版本撤回（或是下架），那麼你可以在套件的 repo 中輸入以下的指令:

`go mod edit -retract=v0.2.0`

這樣一來，就會發現 `go.mod` 檔案變成以下的內容

```
module gopher.live/ue0ddd4a99c02/proverb

go 1.16

// Go proverb was totally wrong
retract v0.2.0
```

這時候，我們可以加上一些註解在 `go.mod` 檔案內，這樣一來其他人要使用的時候，也會出現相關註解。

```
git add -A
$ git commit -q -m "Fix severe error in Go proverb"
$ git push -q origin main
remote: . Processing 1 references        
remote: Processed 1 references in total        
$ git tag v0.3.0
$ git push -q origin v0.3.0
```

透過以上方式，可以將版號推進一號。也已經把正確的內容修正好了。



如果其他人想要拉下有問題的版本，就會出現相關警告。

```
go get gopher.live/ue0ddd4a99c02/proverb@v0.2.0
go: warning: gopher.live/ue0ddd4a99c02/proverb@v0.2.0: retracted by module author: Go proverb was totally wrong
go: to switch to the latest unretracted version, run:
	go get gopher.live/ue0ddd4a99c02/proverb@latestgo get: downgraded gopher.live/ue0ddd4a99c02/proverb v0.3.0 => v0.2.0
```

這樣的方式，就可以透過這個方式來達到撤回版本的流程。 



### 相關疑問：

- 如果沒有執行 `go get `來連接查詢，是不是沒有辦法取得版本撤回的資訊？
  - 沒有錯，目前依舊需要透過 `go get` 或是 `go list` 來取得資料。


## 相關學習資源

<a id="retraction-reference"></a>

- [Using Go Modules](https://blog.golang.org/using-go-modules)
- [Go Dev Playground: Retract Module Versions](https://play-with-go.dev/retract-module-versions_go116_en/)
- [Retract Go Module Versions in Go 1.16](https://golangtutorial.dev/tips/retract-go-module-versions/)

