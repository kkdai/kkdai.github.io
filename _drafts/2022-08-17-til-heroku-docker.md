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

其實 [https://github.com/cshum/imagor](https://github.com/cshum/imagor) 本來已經有 Docker Image 了， 這邊講一些基本導入流程：

### 加上 Heroku 支援:

### 1. 加入新檔案 `app.json` 

```
{
    "name": "imagor",
    "description": "Fast, Docker-ready image processing server in Go with libvips",
    "keywords": [
        "image",
        "resize-images",
        "crop-image",
        "microservice",
        "docker",
        "jpeg",
        "png",
        "libvips"
    ],
    "repository": "https://github.com/cshum/imagor",
    "stack": "container",
    "env": {
        "IMAGOR_UNSAFE": {
            "description": "Use Unsafe mode, default 1 for testing. In production environment, it is highly recommended turning off `IMAGOR_UNSAFE` and setting up URL signature using `IMAGOR_SECRET`, to prevent DDoS attacks that abuse multiple image operations.",
            "required": true,
            "value": "1"
        },
        "IMAGOR_SECRET": {
            "description": "Secret key for URL signature.",
            "required": false
        }
    }
}
```

- 幾個重要的寫一下：
  - `    "repository": "https://github.com/cshum/imagor",`:  讓 Heroku 知道要去哪找其他檔案。
  - `"stack": "container",`：這就是告訴 Heroku 要去找 `Heroku.yml` 這個檔案來繼續接下來流程。
  - `IMAGOR_UNSAFE 這個參數因為這個套件預設要 url signature （可以參考 [這篇說明文章](https://docs.aws.amazon.com/zh_tw/AmazonS3/latest/userguide/PresignedUrlUploadObject.html))

### 2. 加上 heroku.yml 

```
build:
  docker:
    web: heroku/Dockerfile
```

稍後會繼續寫 `heroku/Dockerfile` 的內容。 但是其實這邊除了 `build:` 以外，還有以下功能：

- **參數設定:** 這個例子，設定了一個 ENV `S3_BUCKET` 

  ```
  setup:
    config:
      S3_BUCKET: my-example-bucket
  ```

  

- **其他 Pluging 設定:** 這個例子，起了一個 postgresql 並且用 DATABASE 的 alias 

  ```
  setup:
    addons:
    - plan: heroku-postgresql
      as: DATABASE
  ```

只要參考一下文章 [Building Docker Images with heroku.yml](https://devcenter.heroku.com/articles/build-docker-images-heroku-yml) 可以查到更多關於 `Heroku.yml` 的用法。

### 3. 專屬 Heroku 的 Dockerfile

```
FROM shumc/imagor:latest
LABEL maintainer="Adrian Shum <adrian@cshum.com>"
```

這邊沒有太多預設流程，可以每次 build ，但是我選擇讓 release management 回歸本來 image owner。所以這邊直接引用就可以。

完整的內容可以參考這次 PR  [https://github.com/cshum/imagor/pull/142](https://github.com/cshum/imagor/pull/142)

# 成果：

可以直接到這個 Repo ，按下 Deploy 來部署。 [https://github.com/cshum/imagor](https://github.com/cshum/imagor)

也可以透過以下伺服器來看成果:

- [顯示一個 Gopher 圖片](https://imagor-kkdai.herokuapp.com/unsafe/fit-in/200x200/filters:fill(white)/https://raw.githubusercontent.com/cshum/imagor/master/testdata/gopher.png)
  ![](https://imagor-kkdai.herokuapp.com/unsafe/fit-in/200x200/filters:fill(white)/https://raw.githubusercontent.com/cshum/imagor/master/testdata/gopher.png)

- [那隻跳舞的香蕉](https://imagor-kkdai.herokuapp.com/unsafe/30x40:100x150/filters:fill(cyan)/raw.githubusercontent.com/cshum/imagor/master/testdata/dancing-banana.gif)
  ![](https://imagor-kkdai.herokuapp.com/unsafe/30x40:100x150/filters:fill(cyan)/raw.githubusercontent.com/cshum/imagor/master/testdata/dancing-banana.gif)

- [兩張圖片結合](https://imagor-kkdai.herokuapp.com/unsafe/fit-in/200x150/filters:fill(yellow):watermark(raw.githubusercontent.com/cshum/imagor/master/testdata/gopher-front.png,repeat,bottom,0,40,40)/raw.githubusercontent.com/cshum/imagor/master/testdata/dancing-banana.gif)
  ![](https://imagor-kkdai.herokuapp.com/unsafe/fit-in/200x150/filters:fill(yellow):watermark(raw.githubusercontent.com/cshum/imagor/master/testdata/gopher-front.png,repeat,bottom,0,40,40)/raw.githubusercontent.com/cshum/imagor/master/testdata/dancing-banana.gif)

  

## 相關文章：

- [Building Docker Images with heroku.yml](https://devcenter.heroku.com/articles/build-docker-images-heroku-yml)
- [How to Deploy Docker Container on Heroku? | Part - 2](https://medium.com/featurepreneur/how-to-deploy-docker-container-on-heroku-part-2-eaaaf1027f0b)
- [站在 Docker 的肩膀上，部署任何語言的 Web 應用到 Heroku](https://medium.com/starbugs/deploy-any-web-application-to-heroku-with-docker-b64b9b0eb93)
- 兩套 Golang 寫的圖片編輯伺服器透過 [libvips](https://www.libvips.org/) 
  - [https://github.com/imgproxy/imgproxy](https://github.com/imgproxy/imgproxy)
  - [https://github.com/cshum/imagor](https://github.com/cshum/imagor)
