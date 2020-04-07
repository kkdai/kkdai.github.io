---
layout: post
title: "[TIL] 超好用的 Kubernetes Console tool - c9s/Vikube.vim "
description: ""
category: 
- TodayILearn
tags: ["TIL", "vim", "Kubernetes"]
---



![](https://raw.githubusercontent.com/c9s/vikube.vim/master/assets/01_pod_describe.png)

Kubernetes 相當好用，但是要維運的時候最痛苦的事情，就是要打 `kubectl` 

雖然我超愛 Golang ，而且其實我基本的 IDE 都是使用 VSCode ，但是我一定要跟各位好好推薦這個好工具． Vikube.vim

不論你透過 alias 設定成 `kc` 甚至是 `k` 還是得要記憶一堆指令  ex: kubectl get pod 

其實就算你不是 vim 的愛用者 (畢竟學習曲線太高了) 我還是很推薦你使用這個工具．  

- `:VikubeContextList` 可以開啟你所有連接過的 K8S 集群，透過 `s`  來切換你的集群．
- `:VikubeNodeList` 可以開啟所有的節點清單，`l` 可以看到 結點上面的 logs,   可以幫助你除錯（如果有問題)
- `:VikubePodList` 可以開啟 pod 清單，當然也可以透過 `l` 來看 POD 是否有出現問題．
- `:VikubeTop` 可以開啟 top 來看各個 POD 的使用量

真的很好用... 再也不用擔心打 `kubectl` 或是忘記相關指令了   :p  



