---
layout: post
layout: post
title: "[Docker] 在Mac OSX上使用筆記"
description: ""
category: 
- docker
tags: [windows, docker]
---


![image](https://docs.docker.com/dist/assets/images/logo.png)

## 前言

自從之前把我的Macbook Air上面的空間[加以清理](http://www.evanlin.com/save-disk-space-from-xcode-in-mba/)之後．總算也空出了20~30G，可以好好的練習一下關於docker在[swarm](https://docs.docker.com/swarm/install-w-machine/)與其他在跨機器端的應用． 不過這裡稍微紀錄一些關於Mac上面使用docker的小筆記．


## 一些筆記
### 如何讓Docker Setting記錄在你所有的console

![](https://docs.docker.com/engine/installation/images/mac_docker_host.svg)

由於Docker在MacOSX 是使用VM的方式來架設，其實真正執行docker的機器是你Virtual Box裡面的Linux VM，所以去呼叫docker相關指令一般有兩個方式:

- 透過Docker Quickterminal來跑
- 另外一個也可以透過SSH的方式主要是鏈接ssh: `192.168.99.100:2376` 帳號密碼是 `docker/tcuser`

不過這樣多少有些麻煩，有沒有可能讓我在我的console裡面(比如說iTerm2)裡面的每一個tab可以直接使用docker呢? 方法如下:


(假設你是使用bash) 打開 `~/.bash_profile` 加入這行

```
eval "$(docker-machine env default)"
```

這樣你的預設使用者都可以找到正確的docker socket並且直接使用docker指令 (ex: docker run, docker build ...)

### 如何讓root 使用相同docker環境

一般而言，原本的docker指令是需要透過`sudo docker XXXX`來執行．但是你會發現這件事情在Mac OSX下面會出現問題．

```
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

一查才發現，上面設定的環境參數並沒有在root的環境變數裡面．而且`docker-machine`是找不到原先的設定(docker-machine預設會讀該使用者的docker socket)，所以你不能像上面一樣使用`docker-machine env`來設定環境變數．

必須要依照以下的方式來設定(假設你root也是使用bash，如果不是可以使用`chsh -s /usr/local/bin/bash root`來變更:

- 先在你安裝docker的使用者下執行`docker-machine env`，可能會跑出類似以下的資料:

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/USER/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
```

-  將以上的資料複製起來，編輯到root的`.bash_profile`．也就是執行`sudo su` 再來編輯 `vi ~/.bash_profile`
-  把以上設定放在裡面，並且儲存
-  呼叫`sudo docker xxx` 改成 `sudo -i docker xxx`

如此一來可以正常執行了．不過缺點是你只要用`docker-machine` 新增會刪除`default`的時候，就得重新設定．
