---
layout: post
title: "[學習心得][Golang: Gotip] 快速嚐鮮 Go Proposal 45713 'Muti-Module Workspaces'"
description: ""
category: 
- 學習文件
- TodayILearn
tags: ["Golang", "gotip", "TIL"]
---



![image-20210819214237647](../images/2021/image-20210819214237647.png)

- Proposal: [45713 Workspace Mode](https://go.googlesource.com/proposal/+/master/design/45713-workspace.md) 
- PR: [45713](https://github.com/golang/go/issues/45713) 
- Demo Video: [YouTube](https://www.youtube.com/watch?v=wQglU5aB5NQ)

# 摘要

Vendoring 跟 Dependency Management 是 Golang 一直想要解決的問題，透過了 `go mod` 原本可以管理第一層的套件。透過 `go mod vendor` 你可以下載完相關的依賴的套件在本地端的 `vendor/` ，如果要改上一層的 dependency 就可以直接修改。 但是如果你要改的 Denpedency 上一層跟他的更上一層的 檔案呢？

以往作法可以透過 `go mod edit ` 來一個個修改，但是一但檔案很多的時候就會相當的複雜。 有什麼方式可以快速在 local 做一些確認，也才好去發 PR 到 upstream 去？ 這邊介紹一個正在做最後審核（如果通過了，預計是 go 1.18 的功能： Multi-Module Workspaces )。

Proposal 作者很貼心，還發了有著大狗狗的 Demo Video 。必須說～因為有隻可愛的大狗～我乖乖的把 proposal 看完了。



<iframe width="560" height="315" src="https://www.youtube.com/embed/wQglU5aB5NQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# 如何安裝

```
＃ 取得最新版本的 golang source code
> go install golang.org/dl/gotip@latest

# 下載某個 CL base 的 gotip 
> gotip download dev.cmdgo

# 看一下修改後的相關指令，注意 initwork, editwork ...
> gotip help mod

The commands are:

	download    download modules to local cache
	edit        edit go.mod from tools or scripts
	editwork    edit go.work from tools or scripts
	graph       print module requirement graph
	init        initialize new module in current directory
	initwork    initialize workspace file
	tidy        add missing and remove unused modules
	vendor      make vendored copy of dependencies
	verify      verify dependencies have expected content
	why         explain why packages or modules are needed
```



## Editor (VSCode) 要怎麼改

`[Preference] -> [Setting] -> [Extension]`

![image-20210820143857744](../images/2021/image-20210820143857744.png)

這樣可以讓 VSCode 裡面的



## 相關文章：

<a id="refer"></a>

- [45713 Workspace Mode](https://go.googlesource.com/proposal/+/master/design/45713-workspace.md) 
- [Demo Video YouTube](https://www.youtube.com/watch?v=wQglU5aB5NQ)

