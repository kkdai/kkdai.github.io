---
layout: post
title: "[TIL] 搞清楚三種 Docker SWarm 的差異"
description: ""
category: 
- TodayILearn
tags: ["docker", "TIL"]

---

## 原文

### [Comparing Swarm, Swarmkit and Swarm Mode](https://sreeninet.wordpress.com/2016/07/14/comparing-swarm-swarmkit-and-swarm-mode/)


## 名詞解釋:

- **Docker Swarm**: 最原始的 Docker Swarm 需要透過 `docker --swarm` 來呼叫
- **Docker SwarmKit**: 使用 [Github](https://github.com/docker/swarmkit) 另外一個 Open Source Project `docker/SwarmKit` 做 Clustering orchestration.
- **Docker SwarmNext**: 也就是 Docker 1.12 之後系統內建的 Docker Swarm Mode (或稱為 Docker Swarm V2)

## TL;DR  (直接告訴我差異)

|   |  Docker Swarm |  Swarm Kit  |  Swarm Next (Swarm V2)  |
|---|---|---|---|
| 額外 K/V DB  | 需要 (`progrium/consul`)  | 內建  | 內建  |
|  Security | None  | 內建  | 內建  |
| 額外安裝  | 不需要  | 需要額外安裝 binaries  | 不需要  |
| Extra Service  | None  | Noone  | Routing Mesh, Load Balancer, Service Discovery  |
| Docker-Compose, Docker-Machine 支援  | 支援  |  支援  | 不支援 (目前)  |



## 關於最原始的 Docker Swarm



### 建立 Key-Value Service

```
#Create Docker Machine
docker-machine create -d virtualbox mh-keystore

#Switch to taret docker-machine
eval "$(docker-machine env mh-keystore)"

#Start Consul Service
docker run -d \
    -p "8500:8500" \
    -h "consul" \
    progrium/consul -server -bootstrap
```    

## 關於 SwarmKit 使用流程

請參考我的這篇文章，[[TIL][Docker] Docker SwarmKit 學習紀錄](http://www.evanlin.com/note-docker-swarmkit/)


## 關於 Docker Swarm Next

請參考我的這篇文章，[[TIL] Learning note about Docker Swarm Mode](http://www.evanlin.com/til-2016-07-13/)


## 參考鏈結

- [试用swarmkit](http://www.jingyuyun.com/article/11440.html)
- [[TIL] Learning note about Docker Swarm Mode](http://www.evanlin.com/til-2016-07-13/)