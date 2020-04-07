---
layout: post
title: "[Podcast 分享] Kubernetes Origin, with Joe Beda "
description: ""
category: 
- podcast
- kubernetes
tags: ["Podcast", "kubernetes"]
---



![](https://3.bp.blogspot.com/-NHRrZtTK_n4/WudRjQCeiiI/AAAAAAAAFfg/PTN8wYpfL64S4XcM5NxvxQjpJfXVslJ_QCLcBGAs/s400/kubernetes-podcast-from-google.png)

# 鏈結

## [Google Kubernetes Podcast: Kubernetes Origins, with Joe Beda Link](https://kubernetespodcast.com/episode/012-kubernetes-origins/)

# 前言

聽 Podcast 是我通勤時候最常做的事情，也是讓我可以練習聽力（但是沒練到說？)  的好機會．  這篇是我今天聽到覺得很有趣的事情，本來只想貼到臉書講個幾句，但是想到大家都~~不想~~很忙，所以決定幫大家把重點全部摘錄出來．順便介紹一些重要的角色．

聽完這篇才知道 Heptio 真是個很強大的公司，難怪 Dave Cheney 要離開 Atlassian 去那邊

# 來賓 -  Joe Beda 

![](https://pbs.twimg.com/profile_images/849046681400123392/JdrflzNE_400x400.jpg)

[Joe Beda](https://twitter.com/jbeda), [Craig McLuckie](https://twitter.com/cmcluck) 跟 [Brendan Burns](https://github.com/brendandburns) 是 Kubernetes 當初的三位發起者． 而 Joe Beda 原本更是在 Google 工作過． 曾在微軟 IE 工程師出身的他，後來到了 Google 後就在 Google Talk 與 GCE  的部門開發相關服務． 後來在 Google 期間推動了 Kubernetes 的開源活動．之後離開了 Google 跟 Craig McLuckie 一起創立了 [Heptio](https://heptio.com/) 一間專門幫忙企業來建置 Kubernetes 服務並且開源了許多有用的 Kubernetes 工具的公司．

話說 Kubernetes 三個發起者到頭來都不是 Google 的人． 

- [Joe Beda](https://twitter.com/jbeda) 原本是 GCE 工程師，後來離開變成 Heptio CTO
- [Craig McLuckie](https://twitter.com/cmcluck) 原本是 Google Group PM 後來變成 Heptio CEO
- [Brendan Burns](https://github.com/brendandburns)  很久之前就離開 Google 並且在微軟成為傑出工程師

## 關於 Heptio 的開源工具

- [ksonnet](https://github.com/ksonnet/ksonnet):  一個方便部署 Kubernetes 服務的工具
- [Heptio Ark](https://github.com/heptio/ark):  幫助你備份 Kubernetes 與做災難復原的工具
- [sonobuoy](https://github.com/heptio/sonobuoy): 一個幫助你檢定你安裝的 Kubernetes 網路設定與基礎設定是否符合基本標準． （後來被 [Kubernetes Conformance](https://github.com/cncf/k8s-conformance) 大量採用)
- [contour](https://github.com/heptio/contour): 一個 Kubernetes ingress controller 用在 Lyft's Envoy proxy. 這是我們大家熟悉的 [Dave Cheney](https://github.com/davecheney) 的主要工作
- [gimbal](https://github.com/heptio/gimbal): gimbal 是 ingress load balancing platform 可以在多個 Kubernetes 或是 openstack 上面使用

# 演講內文

這裡我會挑出幾個有趣的問題，將他解釋清楚一點．

## 為何請到 Joe Beda

Kubernetes 在近期剛好歡慶[四週年](https://kubernetes.io/blog/2018/06/06/4-years-of-k8s/)，他們就想找當初的發起人來談談到底為何會有 Kubernetes 這個產品．

## Kubernetes 的起源

當時 [Joe Beda](https://twitter.com/jbeda)  與 [Craig McLuckie](https://twitter.com/cmcluck)  都主要在推廣 GCE  的服務，剛好有新的老闆來於是他們很興奮地展示 GCE 當時的功能． 當時 GCE 最為人津津樂道的就是他們開 機器(VM) 的速度超快，從設定好到機器安裝 OS 到開機只要數十秒鐘． 讓 GCE 團隊的人都很自豪，但是給他們新的老闆展示的時候就沒那麼有興趣了．

那時候他們才發覺到 GCP 要做的服務不僅僅是開機器快而已， 而是機器開好之後如何讓人能夠有效地使用它們． 所以這時候有兩種方向：

- 第一種就是讓 Googler 放棄他們原先在使用 Container (註: Google container 並不是 docker container 而是 Google 自己特製的)，轉而一起來開發 VM 上面的應用．
- 不然就是讓 Google 內部的 container orchestration 工具 [Borg](https://research.google.com/pubs/pub43438.html?hl=es) 來給大家使用．

所以後來的聲浪就是後者，也就是想要打造之後稱為的 GKE (Google Kubernetes Engine) 一個給大眾使用的在 GCP 上面的集群管理系統．但是考量到 [Borg](https://research.google.com/pubs/pub43438.html?hl=es) 是由 C++ 打造而成，裡面充斥著太多 Google  內部的管理機密．加上當時的 Golang 與 Docker正在興起．於是他們參考了 [Borg](https://research.google.com/pubs/pub43438.html?hl=es)  與 [Omega](https://ai.google/research/pubs/pub41684) 的概念，並且透過 Golang 來打造，底層一開始先使用 docker container 所建置而成．

## 談談 Kubernetes 開源這件事

大家或許會認為 Google 很樂意將許多產品開源，從 Android 到 Chromium．但是其實要將 Kubernetes 開源在 Google 內卻有許多的討論，Google 之前曾經發表過 [Map Reduce Paper](https://static.googleusercontent.com/media/research.google.com/zh-TW//archive/mapreduce-osdi04.pdf) 後來被其他公司的工程師看懂後開發出 [Hadoop](http://hadoop.apache.org/) 或是 [Big Table Paper](https://static.googleusercontent.com/media/research.google.com/zh-TW//archive/bigtable-osdi06.pdf) 也被開發成 [HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) 的系統． 但是 Google 都沒有公佈任何原始碼 ． 所以再決定要開源 Kubernetes 的立場上， Google 也決定要更認真的經營社群．

## 許多新的功能被要求加進去 Kubernetes

就像是 Serverless 或是 Machine Learning 的相關運用，都是近幾年 Kubernetes 相當熱門的衍生應用． [Joe Beda](https://twitter.com/jbeda)  說他被太多人問說有想要加上的特殊功能．但是他建議這樣的功能應該寫在系統之外，讓 Kubernetes 專注在精簡的功能．

[Joe Beda](https://twitter.com/jbeda) 也表示，他希望四年可以期待看到 Kubernetes 維持目前 Boring (指的是單純，沒有太多額外功能)而看到越來越多人發展 Kubernetes 相關的應用． 

# Reference Link:

- [Joe Beda on Twitter](https://twitter.com/jbeda)
- [Heptio](https://heptio.com/)
  - [Heptio Blog](https://blog.heptio.com/)
- [4 years of Kubernetes blog post](https://kubernetes.io/blog/2018/06/06/4-years-of-k8s/)
