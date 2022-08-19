---
layout: post
title: "[學習文件] 如何在 Heroku 直接一件部署 deploy Golang Docker Container"
description: ""
category: 
- 學習文件
tags: ["Golang", "Heroku", "Docker"]
---

![image-20220819112137025](../images/2021/image-20220819112137025.png)

## 前言:

我一直蠻喜歡在 Github Repo 直接加上「一鍵部署到 Heroku 」的按鈕，隨著近期不少的 Golang Repo 因為有使用到一些 C 語言相關的底層套件。 不少的服務都有打包成 Docker 的方式來做 Github 的 CICD 。 但是對於只是想要透過 Heroku 快速使用這個套件功能的開發者，如何快速導入 Heroku 的 Deploy on Heroku 就是本文敘述的重點。

# 一個關於圖片編輯的套件 imagor

這次文章所敘述的修改，將透過套件 [https://github.com/cshum/imagor]( https://github.com/cshum/imagor) 作為範例。

## 什麼是 [imagor](https://github.com/cshum/imagor) 

是一個透過 Golang 來直接操作知名的圖片編輯套件 [libvips (用 C 打造的)](https://www.libvips.org/) 的套件，本身已經有 Docker 的打包好的 docker image (`shumc/imagor` ) 。這邊有一套類似的已經商業化的套件為 [Imgproxy](https://github.com/imgproxy/imgproxy) 。

## 使用簡單，功能強大

快速的部署可以透過以下指令：

```
docker run -p 8000:8000 shumc/imagor -imagor-unsafe -imagor-auto-webp
```

並且可以有以下的成果，可以到  [https://github.com/cshum/imagor]( https://github.com/cshum/imagor)  查看。

### 原圖

[![img](https://raw.githubusercontent.com/cshum/imagor/master/testdata/dancing-banana.gif)](https://raw.githubusercontent.com/cshum/imagor/master/testdata/dancing-banana.gif) [<img src="https://raw.githubusercontent.com/cshum/imagor/master/testdata/gopher-front.png" alt="img" style="zoom:33%;" />](https://raw.githubusercontent.com/cshum/imagor/master/testdata/gopher-front.png)

### 合成

[![img](https://raw.githubusercontent.com/cshum/imagor/master/testdata/demo5.gif)](https://raw.githubusercontent.com/cshum/imagor/master/testdata/demo5.gif)

## 修改流程：





# 成果：

可以直接到這個 Repo ，按下 Deploy 來部署。 [https://github.com/cshum/imagor](https://github.com/cshum/imagor)

也可以透過以下伺服器來看成果:



## 相關文章：

- [Building Docker Images with heroku.yml](https://devcenter.heroku.com/articles/build-docker-images-heroku-yml)

- [How to Deploy Docker Container on Heroku? | Part - 2](https://medium.com/featurepreneur/how-to-deploy-docker-container-on-heroku-part-2-eaaaf1027f0b)

- [站在 Docker 的肩膀上，部署任何語言的 Web 應用到 Heroku](https://medium.com/starbugs/deploy-any-web-application-to-heroku-with-docker-b64b9b0eb93)

- 兩套 Golang 寫的圖片編輯伺服器透過 [libvips](https://www.libvips.org/) 
  - [https://github.com/imgproxy/imgproxy](https://github.com/imgproxy/imgproxy)
  - [https://github.com/cshum/imagor](https://github.com/cshum/imagor)

- 
