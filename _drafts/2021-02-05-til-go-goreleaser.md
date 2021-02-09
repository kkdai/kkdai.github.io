---
layout: post
title: "[TIL][Golang] 打包你用 Golang 寫的 CLI 工具 (Command Line Tool)，並且搭配 Github Actions 準備 Changelogs "
description: ""
category: 
- TodayILearn
tags: ["Golang", "DevOps"]
---



![](https://avatars2.githubusercontent.com/u/24697112?v=3&s=200)

## 前言:

使用 Golang 來開發小工具最方便的方式，就是可以很快速將程式碼託管在 github.com 。 並且透過 Golang 的跨平台編譯的工具，可以快速打包出各種平台（Windows, Linux 跟 Mac 平台）的執行檔。

那麼有沒有方式可以直接在 github.com 發行新的版本後，直接就打包好所有執行檔變且幫你把 Changelog 都打好呢？

## TL;DR 

本篇文將要介紹：

- 如何快速打包你的 Golang Console App (Command-line App)
- 如何整合到 GitHub action
  - 發行新的版本 (Release) 的時候，就直接打包好新版本
  - 並且可以自動幫你打好所有 Release Note （包含 Changelog )



## 以前要如何打包你的 Golang CLI ?

在以前的時候，曾經有出過一個很方便可以快速打包所有平台執行檔案的小工具。 Gox 就是一個很方便的小工具：

### GOX 快速快平台打包工具（以前）

https://github.com/mitchellh/gox 

```
$ go get github.com/mitchellh/gox
...
$ gox -h
...
```

就這麼簡單，就可以快速編譯跨平台的工具。 其實因為 Golang 從 1.5 之後就支援跨平台編譯。可以參考這篇 Dave Cheney 的文章。

- [Cross compilation with Go 1.5](https://dave.cheney.net/2015/08/22/cross-compilation-with-go-1-5)

- [An introduction to cross compilation with Go](https://dave.cheney.net/2012/09/08/an-introduction-to-cross-compilation-with-go)

其實跨平台編譯透過 

```
env GOOS=linux GOARCH=arm go build -v github.com/constabulary/gb/cmd/gb
```

透過這個方式就可以快速的打包你的工具，所以其實後來 gox 就也比較沒人在用。



## GoReleaser 一個好用的打包發佈的工具

![](https://goreleaser.com/static/logo.png)

後來我也看到 https://github.com/kkdai/youtube 一起在打造的夥伴們有導入 [GoReleaser](https://goreleaser.com/) 。 看了一下，發現還真的蠻好用的。



## 整合進 github action

## 如何產生 Changelog

記得不要打任何 Describe 在你的 release

記得不要打任何 Describe 在你的 release

記得不要打任何 Describe 在你的 release



## Github Repo 

https://github.com/kkdai/go-cli-template






## 相關文章：

- GoReleaser Quick Star https://goreleaser.com/quick-start/
- [🚀 GitHub Action for release your Go projects as fast and easily as possible](https://dev.to/koddr/github-action-for-release-your-go-projects-as-fast-and-easily-as-possible-20a2)
- [Golang Github Actions Starter](https://github.com/actions/starter-workflows/blob/c59b62dee0eae1f9f368b7011cf05c2fc42cf084/ci/go.yml)
