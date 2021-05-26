---
layout: post
title: "[TIL][Golang] 使用 GoReleaser 一次打包多個與多種執行檔"
description: ""
category: 
- TodayILearn
tags: ["Golang", "DevOps", "CICD"]
---



<img src="https://goreleaser.com/static/logo.png" width="600px">

## 前言:

上個月曾經有一篇文章有提到 [使用 GoReleaser 打包你用 Golang 寫的 CLI (Command Line Tool)，並且搭配 Github Actions 準備 Changelogs](http://www.evanlin.com/til-go-goreleaser/) ，透過 [GoReleaser](https://goreleaser.com/) 可以跟 Github Action 整合之外，還可以幫你撰寫 Changelogs 讓版本管控上變得更加的簡單與方便。

但是隨著服務與產品的應用變廣，會有更多的客製化需求發生。那麼該如何讓你的 Github Action 能夠有更多樣的自訂設定呢？



## 基本需求： GoReleaser with Github Action

大家可以參考前一篇文章，也可以快速學習本篇。

<script src="https://gist.github.com/kkdai/d32ea8f7f99a7097e429b194d2c58c56.js"></script>

把這個建立成檔案在 `.github/workflows/release_build.yml` 就可以了。  這個其實比較適合 main.go 直接放在 github repo 下的，可以參考  <https://github.com/kkdai/go-cli-template> 的專案形式。



## main.go 在子目錄 (sub-folder) Main program in sub-folder

通常在開發一些套件的時候，有時候我們主要 Repository 會是相關的開發套件 (Package) 而會將 CLI 的部分放在 `cmd/xxx_cli` 的資料夾中。需要安裝的時候再跑 `go install xxx.com/xxx/uuu/cmd/xxx_cli` 來安裝。

如果這時候你的 `main.go` 並不在 repo 的主目錄下，而是在 `cmd/test_cli` 的目錄下。 這個時候你就會發生錯誤。

![](https://user-images.githubusercontent.com/2252691/119249210-4d062100-bbc9-11eb-9ee1-a60ec8dcc820.png)

```
goreleaser error: dones not contain a main function
```

這時候要如何修改呢?

<script src="https://gist.github.com/kkdai/34e2167df032a859945b487593465bdb.js"></script>

依照上面顯示，主要修改部分就在於 `workdir: ./cmd/test_cli` 放在 Steps Run GoReleaser 裡面的 `with:`。 這樣就可以正常的編譯出執行檔案，並且更新 changelogs 。








## 相關文章：

- GoReleaser Quick Star https://goreleaser.com/quick-start/
- [🚀 GitHub Action for release your Go projects as fast and easily as possible](https://dev.to/koddr/github-action-for-release-your-go-projects-as-fast-and-easily-as-possible-20a2)
- [Golang Github Actions Starter](https://github.com/actions/starter-workflows/blob/c59b62dee0eae1f9f368b7011cf05c2fc42cf084/ci/go.yml)
- [GoReleaser Builds Configuration](https://goreleaser.com/customization/build/)

