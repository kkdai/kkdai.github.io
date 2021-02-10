---
layout: post
title: "[TIL][Golang] 打包你用 Golang 寫的 CLI 工具 (Command Line Tool)，並且搭配 Github Actions 準備 Changelogs "
description: ""
category: 
- TodayILearn
tags: ["Golang", "DevOps", "CICD"]
---



![](https://avatars2.githubusercontent.com/u/24697112?v=3&s=200)

## 前言:

使用 Golang 來開發小工具最方便的方式，就是可以很快速將程式碼託管在 github.com 。 並且透過 Golang 的跨平台編譯的工具，可以快速打包出各種平台（Windows, Linux 跟 Mac 平台）的執行檔。

那麼有沒有方式可以直接在 github.com 發行新的版本後，直接就打包好所有執行檔變且幫你把 Changelog 都打好呢？

## TL;DR 

本篇文將要介紹：

- <a href="#gox">以前如何打包跨平台的套件 (GOX)</a>

- <a href="#goreleasder">新的打包套件 GoReleaser</a> 
- <a href="#github_action">如何整合到 GitHub action</a>
  - 發行新的版本 (Release) 的時候，就直接打包好新版本
  - 並且可以自動幫你打好所有 Release Note （包含 Changelog )







## 以前要如何打包你的 Golang CLI ?

<a id="gox"></a>

在以前的時候，曾經有出過一個很方便可以快速打包所有平台執行檔案的小工具。 Gox 就是一個很方便的小工具：

### GOX 快速快平台打包工具（以前）

<https://github.com/mitchellh/gox>

```
$ go get github.com/mitchellh/gox
...
$ gox -h
...
```

就這麼簡單，就可以快速編譯跨平台的工具。 其實因為 Golang 從 1.5 之後就支援跨平台編譯。 其實跨平台編譯透過 

```
env GOOS=linux GOARCH=arm go build -v github.com/constabulary/gb/cmd/gb
```

透過這個方式就可以快速的打包你的工具，所以其實後來 gox 就也比較沒人在用。

#### 需要注意地方：

- AMD 64 只能編譯 AMD64
- 如果要編輯 ARM 就需要使用到 ARM 版本的處理好才可以。



#### 關於跨平台打包（編譯）更多的文章:

關於跨平台編譯更多的詳細敘述，可以參考這篇 Dave Cheney 的文章。

- [Cross compilation with Go 1.5](https://dave.cheney.net/2015/08/22/cross-compilation-with-go-1-5)

- [An introduction to cross compilation with Go](https://dave.cheney.net/2012/09/08/an-introduction-to-cross-compilation-with-go)





## GoReleaser 一個好用的打包發佈的工具

<a id="goreleaser"></a>

![](https://goreleaser.com/static/logo.png)

後來我也看到 https://github.com/kkdai/youtube 一起在打造的夥伴們有導入 [GoReleaser](https://goreleaser.com/) 。 看了一下，發現還真的蠻好用的。

這裡簡單介紹一下， GoReleaser 有做哪些事情:

- 幫助你一次透過多平台打包套件
- 可以深度整合 Github 跟 Gitlab 讓你直接發佈整個產品提供下載
- 可以幫忙整理出 ChangeLog | 可以幫忙整理出 ChangeLog | 可以幫忙整理出 ChangeLog (懶人福星)
- 整合 Docker 相關功能（打包 Docker Image) 

### GoReleaser 的安裝方式

- `brew install goreleaser/tap/goreleaser`
- `curl -sfL https://install.goreleaser.com/github.com/goreleaser/goreleaser.sh | sh`

### 如何使用 GoReleaser

參考  [GoReleaser QuickStar](https://goreleaser.com/quick-start/) 

- `goreleaser init` 來產生 `.goreleaser.yml` 的樣板檔案

- 來測試一下打包 `goreleaser --snapshot --skip-publish --rm-dist`

- 設定環境變數，讓你可以跟 Github 整合 `export GITHUB_TOKEN="YOUR_GH_TOKEN"`

  - Github Token 產生方式，去這一頁. https://github.com/settings/tokens/new

- 如果需要發布新版本，依照以下兩個步驟:

  - ```
    git tag -a v0.1.0 -m "First release"
    git push origin v0.1.0
    ```

  - `goreleaser release`

- 就會在 Github release 直接產生一個 Release ，並且把 ChangeLog 都包進去

![](https://img.carlosbecker.dev/goreleaser-github.png)

是不是很方便？



### 可能會有的問題



## 整合進 github action

<a id="github_action"></a>

## 如何產生 Changelog

記得不要打任何 Describe 在你的 release

記得不要打任何 Describe 在你的 release

記得不要打任何 Describe 在你的 release



## 想找一個打包好的樣板，試試看？ Github Command-line Template Repo  

https://github.com/kkdai/go-cli-template






## 相關文章：

- GoReleaser Quick Star https://goreleaser.com/quick-start/
- [🚀 GitHub Action for release your Go projects as fast and easily as possible](https://dev.to/koddr/github-action-for-release-your-go-projects-as-fast-and-easily-as-possible-20a2)
- [Golang Github Actions Starter](https://github.com/actions/starter-workflows/blob/c59b62dee0eae1f9f368b7011cf05c2fc42cf084/ci/go.yml)
