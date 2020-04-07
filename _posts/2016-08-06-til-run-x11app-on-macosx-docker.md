---
layout: post
title: "[TIL] 在 MacOSX 上面透過 Docker 來跑 X11 原生視窗 App"
description: ""
category: 
- TodayILearn
tags: ["docker", "TIL", "macosx", "x11"]

---

## 原文

### [Bring Linux apps to the Mac Desktop with Docker](http://blog.alexellis.io/linux-desktop-on-mac/)

## 好處:


- 某些 App 只出 Linux App 版本，卻沒有 MacOSX
- 透過 sandbox 的方式執行程式

## 相關準備:

### 先裝 X11 Client - xquartz

```
brew install Caskroom/cask/xquartz
```

### 安裝 TCP/UDP mapping 工具 SOCAT

```
brew install socat
```

### 撰寫相關的 Dockerfile

```
vi Dockerfile
```

內容直接複製貼上...

<script src="https://gist.github.com/kkdai/769204b28c8b16c4eb6bf0f9c4756352.js"></script>


## 開始吧

### 先下載這次範例程式 slack linux 版本

```
wget https://downloads.slack-edge.com/linux_releases/slack-desktop-2.1.0-amd64.deb
```

### 先在另外一個 teminal 跑 SOCAT

```
socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"
```
 
記得不要關掉，這是對應 X11 Client/Server 對應的部分 
 
### 編譯 Docker Image

```
docker build -t slack:2.1.0 .
```

### 跑起來吧

```
docker run -e DISPLAY=192.168.0.15:0 --name slack -d slack:2.1.0
```


