---
layout: post
title: "[學習文件] 如何透過 GCP STT (Speech To Text) 與 LINE Video Flex Message 來打造幫助長輩的影片翻譯（自動輸出文字）聊天機器人"
description: ""
category: 
- 學習文件
tags: ["GCP", "Hugo", "Blog"]
---

<img src="../images/2021/image-20220407212250046.png" alt="image-20220407212250046" style="zoom:50%;" />

# 前言:

大家好，我是 LINE 台灣的資深技術推廣工程師的 Evan Lin 。 

# Demo (展示) :

# 開發流程記錄：



## 如何取得 GCS 對外的公開位置

### 打開分享權限

建議透過介面打開來權限

- [到 Console](https://console.cloud.google.com/storage/browser)  到擁有 Google Cloud Storage 的專案
- 透過 More Button 打開 Edit Access

![image-20220408105439200](../images/2021/image-20220408105439200.png)

### 注意事項：
- 如果你想透過 code 來改權限，請不要使用一個個檔案來打開。會出錯。 （[這段在最近使用就會有問題](https://github.com/GoogleCloudPlatform/golang-samples/blob/HEAD/storage/objects/make_public.go)）
- 打開一次資料夾權限，之後不需要再調整。

### 取得公開對外的檔案位置






# 相關文章：

- [GCS Doc: Make data public](https://cloud.google.com/storage/docs/access-control/making-data-public)
- [iThome 鐵人賽 - Day 23 Google Cloud Speech-to-Text - 子系列最終章](https://ithelp.ithome.com.tw/articles/10223077?sc=iThelpR)
- [[學習文件\] 如何在 Heroku 上面使用透過 Golang 來存取 Google Cloud 服務](http://www.evanlin.com/til-heroku-gcp-key/)
- [GCP: Speech-to-Text client libraries](https://cloud.google.com/speech-to-text/docs/libraries?hl=zh-tw#client-libraries-resources-go)
-  [Stackoverflow: How do I retrieve a file from GCS using a browser if all I know is the gs scheme URI?](https://stackoverflow.com/questions/34214491/how-do-i-retrieve-a-file-from-gcs-using-a-browser-if-all-i-know-is-the-gs-scheme)

  

   
