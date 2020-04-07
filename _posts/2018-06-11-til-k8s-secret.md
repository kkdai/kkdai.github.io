---
layout: post
title: "[TIL][Kubernetes] 開發一個 Kubernetes Secret 相關應用筆記"
description: ""
category: 
- TodayILearn
- Kubernetes
tags: ["TIL", "kubernetes", "devops"]
---



# 前提:

通常在 Kubernetes 裡面要將設定檔寫入的方式有兩種:

當資料不敏感（具有保密資料) 的時候，我們會使用 ConfigMap ，當你要儲存比較敏感的資料 (service account, password, 私密資料) 就要使用 Secret

# 簡單介紹 Kubernetes Secret:

<iframe src="//www.slideshare.net/slideshow/embed_code/key/1p7cKqhRzz0rSL" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/EvansLin/kubernetes-secret-introduction" title="Kubernetes secret introduction" target="_blank">Kubernetes secret introduction</a> </strong> from <strong><a href="https://www.slideshare.net/EvansLin" target="_blank">Evan Lin</a></strong> </div>

不囉唆，看之前整理好的投影片．



# 不分類小筆記:

## Secret 設定相關: 



- Kubenetes Secret 無法在 Entrypoint 就讀取到，必須等到 POD 跑起來後執行．
- 如果要透過 Secret 來讀取 Kubernetes service account 的相關設定，建議跑在 Command, Args 裡面設定． 

## Kubernetes 執行 Commands 與 Args:



- Kubernetes yaml 的 ARGs 主要拿來跑參數用，如果想要作為 command 跑 `sh` 的內容，要使用 `&&` 就不能分散在 Args 之中．

## 如何透過 Secret 處理敏感資料 (密碼,  Key, Kubenetes 設定文件)



- 如果要將敏感的 json data (configuration, service accoutn, password config) 寫入 secret volume 可以透過 base64 來 encode
- 透過 base64 encode 的 Secret 資料，不需要再跑 decode ．可以直接在 Pod 裡面讀取．