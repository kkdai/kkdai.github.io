---
layout: post
title: "[投影片分享] Introducing Go channel and pipeline"
description: ""
category: 
- golang
tags: ["golang"]
---



![](https://talks.golang.org/2012/waza/gophercomplex1.jpg)

# 鏈結

## [投影片](https://go-talks.appspot.com/github.com/kkdai/GolangTalks/introduction-channel/gochannel.slide#1)

# 前言



前幾天在公司的內訓投影片，簡單介紹如何應用 Buffered Channel 跟 Unbuffered Channel 並且導入在 data pipeline 的應用

結果公司內部激發 Go 核心討論.. 讚讚

# 內容簡介

主要是講解 Go channel 跟 pipeline 的運用． 其中內容會帶到 buffered channel 與 unbuffered 的運用場景． 並且解釋 fan-in 與 fan-out ．最後會帶到 pipeline 寫法與介紹 go-kit 裡面 endpoint 的用法．

# 補充

這個投影片是透過 [Present](https://godoc.org/golang.org/x/tools/present) 所製作的，這個算是官方常在 GopherCon 使用的投影片工具．主要有幾個特點:

- 簡潔
- 可以顯示程式碼
- 還可以線上修改與執行

幾個點需要補充一下:

- 線上修改 [Present](https://godoc.org/golang.org/x/tools/present) 的程式碼需要使用 "shift + enter" 來做換行．不然會有問題．

- 除了顯示程式碼，也可以顯示圖片，但是常常需要調整顯示大小

   