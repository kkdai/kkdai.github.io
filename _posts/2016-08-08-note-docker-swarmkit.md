---
layout: post
title: "[TIL][Docker] Docker SwarmKit 學習紀錄"
description: ""
category: 
- TodayILearn
tags: ["docker", "TIL"]

---

![](https://cdn-images-1.medium.com/max/800/1*KiETLgooPS5XMotv0Blisg.png)

## 前言

其實之前就跑過一次，不過沒有記錄下來詳細的指令．這次為了比較 Docker Swarm ， Docker Swarm Kit 還有 Docker Swarm Mode，又把整個流程跑了一次．

稍微做個紀錄．

## SwarmKit 使用流程



### 安裝 SwarmKit 

由於 Open Source Project，需要另外安裝．

```
go get -u github.com/docker/swarmkit

cd $GOPATH/src/github.com/docker/swarmkit
make binaries
mv bin/* $GOBIN
```

**注意**: 如果你不是使用 Brew 安裝 Golang 你可能不會有 `$GOBIN` 參數，可以改到 `$GOPATH/bin`

### 建立 Cluster Master Node

```
swarmd -d /tmp/node-1 --listen-control-api /tmp/manager1/swarm.sock --hostname node-1
```

要注意解釋一下，其中 `/tmp/manager1/swarm.sock` 是 SwarmKit socket 的位址．如果其他的 node 要加入，一定要將環境變數加入．

```
export SWARM_SOCKET=/tmp/manager1/swarm.sock
```

然後我們來查詢，現在 Master 的 token （在新的 console)

```
export SWARM_SOCKET=/tmp/manager1/swarm.sock

swarmctl cluster inspect default

>ID          : 1piq7f9tr1xlmnui4xhjhsafi
>Name        : default
>Orchestration settings:
>  Task history entries: 5
>Dispatcher settings:
>  Dispatcher heartbeat period: 5s
>Certificate Authority settings:
>  Certificate Validity Duration: 2160h0m0s
>  Join Tokens:
>    Worker: >SWMTKN-1-1wttj6u10f9fueptptma9ohf99zcxt0gia1wt3a5odphi6nt1f-c4y428p7wwr23efwo4xw6qiwz
>    Manager: >SWMTKN-1-1wttj6u10f9fueptptma9ohf99zcxt0gia1wt3a5odphi6nt1f-cdh5ucqp1xjvh3pp1rvs0two4
```

### 建立 Cluster Worker Node

#### 節點 2 (Node2)

```
swarmd -d /tmp/node-2 --hostname node-2 --join-addr 127.0.0.1:4242 --join-token SWMTKN-1-1wttj6u10f9fueptptma9ohf99zcxt0gia1wt3a5odphi6nt1f-c4y428p7wwr23efwo4xw6qiwz
```

其中注意， `--join-token` 一定要加入，不然會找不到．請使用查詢到的，勿用到我提供的 :p

#### 節點 3 (Node3)

```
swarmd -d /tmp/node-3 --hostname node-3 --join-addr 127.0.0.1:4242 --join-token SWMTKN-1-1wttj6u10f9fueptptma9ohf99zcxt0gia1wt3a5odphi6nt1f-c4y428p7wwr23efwo4xw6qiwz
```

### 確認節點都有建立成功

```
export SWARM_SOCKET=/tmp/manager1/swarm.sock                                                                           swarmctl node ls                                                                                                       

二  8/ 9 03:02:10 2016
ID                         Name    Membership  Status  Availability  Manager Status
--                         ----    ----------  ------  ------------  --------------
01kgkezj7wcwij5qcp78raz1i  node-1  ACCEPTED    READY   ACTIVE        REACHABLE *
2ocq4129a4y2nq23hajdsqv0t  node-3  ACCEPTED    READY   ACTIVE
b75cdesh7to4lb35wg17ul12x  node-2  ACCEPTED    READY   ACTIVE
```

### 建立一個 Service  Redis 3.0.5

```
swarmctl service create --name redis --image redis:3.0.5
```

### 確認 Service 有成功


```
swarmctl service ls

ID                         Name   Image        Replicas
--                         ----   -----        --------
94h7xat76kjd50as63f5qtsex  redis  redis:3.0.5  1/1
```

觀察 Service 細節

```
swarmctl service inspect redis

ID                : 94h7xat76kjd50as63f5qtsex
Name              : redis
Replicas          : 1/1
Template
 Container
  Image           : redis:3.0.5

Task ID                      Service    Slot    Image          Desired State    Last State              Node
-------                      -------    ----    -----          -------------    ----------              ----
49d41k08a4bifkeo67xv00f5c    redis      1       redis:3.0.5    RUNNING          RUNNING 1 minute ago    node-1
```

### Scale Service using SwarmKit

```
swarmctl service update redis --replicas 6

swarmctl service ls                                                                                                    

ID                         Name   Image        Replicas
--                         ----   -----        --------
94h7xat76kjd50as63f5qtsex  redis  redis:3.0.5  1/6


swarmctl service ls                                                                                                    

ID                         Name   Image        Replicas
--                         ----   -----        --------
94h7xat76kjd50as63f5qtsex  redis  redis:3.0.5  6/6
```

檢查更多詳細的參數

```
swarmctl service inspect redis                                                                                         二  8/ 9 10:02:45 2016
ID                : 94h7xat76kjd50as63f5qtsex
Name              : redis
Replicas          : 6/6
Template
 Container
  Image           : redis:3.0.5

Task ID                      Service    Slot    Image          Desired State    Last State                Node
-------                      -------    ----    -----          -------------    ----------                ----
49d41k08a4bifkeo67xv00f5c    redis      1       redis:3.0.5    RUNNING          RUNNING 2 minutes ago     node-1
83av0fuyn7wqqk32fvpmrtu2o    redis      2       redis:3.0.5    RUNNING          RUNNING 27 seconds ago    node-3
9lvd4xtwxlbskktxa7asa04fe    redis      3       redis:3.0.5    RUNNING          RUNNING 28 seconds ago    node-2
74ca5vzx1wbviycrmuq8tb1mi    redis      4       redis:3.0.5    RUNNING          RUNNING 27 seconds ago    node-2
6rwgiz70onihivlex5jdi96fj    redis      5       redis:3.0.5    RUNNING          RUNNING 27 seconds ago    node-1
brp9u9fkk26xs6cmrseye4hcu    redis      6       redis:3.0.5    RUNNING          RUNNING 27 seconds ago    node-3
```

### 更新服務到 3.0.6

預設並沒有使用 Rolling Update ，會直接更新到最新版本．

```
swarmctl service update redis --image redis:3.0.6                                                                      

94h7xat76kjd50as63f5qtsex


swarmctl service inspect redis
ID                : 89831rq7oplzp6oqcqoswquf2
Name              : redis
Replicas          : 6
Template
 Container
  Image           : redis:3.0.6

Task ID                      Service    Instance    Image          Desired State    Last State                Node
-------                      -------    --------    -----          -------------    ----------                ----
7947mlunwz2dmlet3c7h84ln3    redis      1           redis:3.0.6    RUNNING          RUNNING 34 seconds ago    node-3
56rcujrassh7tlljp3k76etyw    redis      2           redis:3.0.6    RUNNING          RUNNING 34 seconds ago    node-1
8l7bwrduq80pkq9tu4bsd95p4    redis      3           redis:3.0.6    RUNNING          RUNNING 36 seconds ago    node-2
3xb1jxytdo07mqccadt06rgi0    redis      4           redis:3.0.6    RUNNING          RUNNING 34 seconds ago    node-1
16aate5akcimsye9cp5xis1ih    redis      5           redis:3.0.6    RUNNING          RUNNING 34 seconds ago    node-2
dws408a3gz0zx0bygq3aj0ztk    redis      6           redis:3.0.6    RUNNING          RUNNING 34 seconds ago    node-3
```

如果要使用 Rolling Update ，每隔 10 秒更新 2 台機器

```
swarmctl service update redis --image redis:3.0.7 --update-parallelism 2 --update-delay 10s

swarmctl service inspect redis                                                                                         二  8/ 9 10:14:40 2016
ID                   : 94h7xat76kjd50as63f5qtsex
Name                 : redis
Replicas             : 4/6
Update Status
 State               : UPDATING
 Started             : 14 seconds ago
 Message             : update in progress
Template
 Container
  Image              : redis:3.0.7

Task ID                      Service    Slot    Image          Desired State    Last State                  Node
-------                      -------    ----    -----          -------------    ----------                  ----
3lbgomkdkfszohl0jkhy7bgwp    redis      1       redis:3.0.7    RUNNING          PREPARING 13 seconds ago    node-2
6db9gj8ssnfxhydtk00fgn93x    redis      2       redis:3.0.6    RUNNING          RUNNING 10 minutes ago      node-2
dgq6iwt0eh951gpe7bd7kxmcf    redis      3       redis:3.0.6    RUNNING          RUNNING 10 minutes ago      node-2
4rhy2jd7ecu968e0p51wohdn5    redis      4       redis:3.0.6    RUNNING          RUNNING 10 minutes ago      node-1
61sb7zev74d9jzh5vvud1vy4z    redis      5       redis:3.0.6    RUNNING          RUNNING 10 minutes ago      node-1
bdy30kw7zq8mytmo6yzdqjm5d    redis      6       redis:3.0.7    RUNNING          PREPARING 13 seconds ago    node-3
```


## 參考鏈結

- [试用swarmkit](http://www.jingyuyun.com/article/11440.html)
- [[TIL] Learning note about Docker Swarm Mode](http://www.evanlin.com/til-2016-07-13/)
- [SwarmKit Github](https://github.com/docker/swarmkit)