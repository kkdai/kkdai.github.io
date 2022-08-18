---
layout: post
title: "[學習文件] 如何在 Heroku 上面使用透過 Golang 來存取 Google Cloud 服務"
description: ""
category: 
- 學習文件
tags: ["GCP", "Hugo", "Blog", "golang", "Heroku"]
---

![image-20220321200254901](../images/2021/image-20220321200254901.png)

## 前言:

以往的時候，如果要使用 Google Cloud 相關的 API (比如說: Google Cloud Storage) 存取 API ，只能透過 JSON 檔案來操作。但是如果你想要放上 Heroku 的時候，就必須要放上 GitHub ，這個時候就很容易不小心誤放 JSON 檔案而被機器人掃走而盜用。

本篇文章將告訴你，透過環境變數跟 Golang Buildpack 可以幫助你在安全無慮的狀況下部署 Herokuu 專案，也可以開源到 Github 。

## 流程：

其實這一篇 [Adding Google Cloud Credentials to Heroku](https://devdojo.com/bryanborge/adding-google-cloud-credentials-to-heroku) 的內容算是蠻完整的，但是裡面的 buildpack 最近用起來怪怪的。

1. 開啟一個專屬的 services account

   1. 透過 Google Cloud Console https://cloud.google.com/storage/docs/reference/libraries#create-service-account-console

      ![image-20220321204811895](../images/2021/image-20220321204811895.png)

2. 設定好需要的權限就好： (圖片是需要 Cloud Storage 的建立者)

   ![image-20220321205120799](../images/2021/image-20220321205120799.png)

3. 下載你的 JSON Key (檔案請妥善儲存)

   ![image-20220321205220312](../images/2021/image-20220321205220312.png)

4. 透過 golang build pack 新增

   ```
   heroku buildpacks:set https://github.com/gerywahyunugraha/heroku-google-application-credentials-buildpack -a your-app-name
   ```

5. 接下來將 JSON 檔案參數填寫進 Heroku 設定

![image-20220321204425338](../images/2021/image-20220321204425338.png)

```
GOOGLE_APPLICATION_CREDENTIALS: google-credentials.json (固定)
GOOGLE_CREDENTIALS: 裡面放 JSON 完整內容。
```

5. 相關程式部分:

<script src="https://gist.github.com/kkdai/96a7c3a9c1b74d40330c1b132b023726.js"></script>


## 相關文章：

- [Adding Google Cloud Credentials to Heroku](https://devdojo.com/bryanborge/adding-google-cloud-credentials-to-heroku)
- [Stackoverflow: How to use Google API credentials json on Heroku?](https://stackoverflow.com/questions/47446480/how-to-use-google-api-credentials-json-on-heroku)
- [Simple Golang API File Uploader Using Google Cloud Storage](https://adityarama1210.medium.com/simple-golang-api-uploader-using-google-cloud-storage-3d5e45df74a5)
