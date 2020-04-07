---
layout: post
layout: post
title: "[Docker] 拆解我的網路服務"
description: ""
category: 
- docker
tags: [docker, go]
---

![](http://i.imgur.com/aOSGvV7.jpg)

## 前言:

講了Docker半天，這一篇主要是記錄一下我把在用的一些Web Service全部放進Docker的紀錄，順便看看有沒有什麼地雷怕踩到的． 其實將自己的服務全部轉成Dockerize Services最重要的重點就是要能夠清楚了解`--link`的作用．

## 原先架構

稍微解釋一下原先架構:

- Go RPC:
	- 主要處理接受一些API，連接MySQL
- MySQL:
	- 主要的資料乘載
- PHP:
	- 裡面有數個Web Services，主要也是連接PHP


### 先從MySQL資料庫開始

資料庫沒有太多困難，由於可能會有資料庫的版本不同，所以做法會是:

- 啟動 MySQL Docker Container (版本自選或是最新)
- 導入資料庫SQL (空白或是有資料)


```bash
docker run --name my-db -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
```

這是一個基本的啟動最新版MySQL的Docker指令，其中也預設了root密碼． 可以使用`-p 3306:3306`來開放port出來給其他server連接．不過由於我們要使用`--link`，所以我們不需要用到export port來用．等等透過Go RPC Server來使用，


### 整合Go RPC Server

這邊其實不討論怎麼build go binary，而是假設你已經有個在[ubuntu](https://hub.docker.com/_/ubuntu/)上面compile好的binary．

```bash
docker run -p 8088:8088 --link my-db -it -v $BiaryADD:/rpc ubuntu /rpc/go-rpc-server
```

這一段程式碼會去執行`ubuntu`基本的image，主要是透過`/rpc/go-rpc-server`來執行已經build好的Go RPC Server binary．

### 開始串連各個microservixes，必須搞懂`--link` 

如果你要用`--link my-db`在新的container上的話，那麼你的Go DB Connection 應該要類似以下的`Go`：

```go
sql.Open("mysql", "root:PW@my-db/DB_NAME")
```

重點就是你要把你要連接的target連到`my-db`而不是localhost．(因為container的localhost會是在docker裡面，而不是那台機器本身)

根據`link`的docker [spec有提到](https://docs.docker.com/engine/userguide/networking/default_network/dockerlinks/)，其實`link`就是做了兩件事情:

- 更新 container ip 在 /etc/hosts
- 設定好系統參數 (讓你可以直接使用`my-db`)

### 整合PHP Web Server

這裡使用的是[tumtumcloud/apache-php](https://github.com/tutumcloud/apache-php)這個image．不過我有稍微修改，一併把外面PHP位置放進去．

```bash
docker run -v /YOUR_PHP_ADD:/app --link my-db
```

透過這個方式，只要你的PHP設定是連接 `$db['host']="my-db"` 就可以了．


### 整在一起放在docker-compose吧

透過以上三個services，其實我們可以寫在同一個`docker-compose.yaml`然後一次啟動．檔案可能是類似以下的方式:

```
go-app:
  build: ./rpc/.
  ports:
    - 8088:8088
  volumes:
    - ./GO_RPC:/rpc
  links:
    - atc-db
  command: /rpc/run_rpc.sh
web-app:
  build: ./apache-php/.
  volumes:
    - ./apache-php/php:/app
  links:
    - my-db
  ports:
    - 80:80
my-db:
  image: mysql
  volumes:
    - ./db/db_data:/var/lib/mysql
  env_file: db.env
```  


稍微解釋一下，主要這邊有三個服務 `go-app`, `web-app`跟`my-db`．就像前面提到的一樣，只是把每個設定不論是`link`或是`volumes`或是`env_file`甚至是`command`都寫入了`docker-compose.yaml`．他就會依照相依關係來啟動個別microservices

1. `my-db` DB services
2. `go-app` Web Go RPC services
3. `web-app` PHP Apache services

這樣大概就把整個有在使用的服務串接成三個container來使用．算是第一個階段完成，有更多的services等著要去dockerized．並且也可以考慮導入swarm來分開在不同的host上面．

 
