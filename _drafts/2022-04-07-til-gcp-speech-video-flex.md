---
layout: post
title: "[學習文件] 如何透過 GCP STT (Speech To Text) 與 LINE Video Flex Message 來打造幫助長輩的影片翻譯（自動輸出文字）聊天機器人"
description: ""
category: 
- 學習文件
tags: ["GCP", "golfing", "Blog"]
---

<img src="../images/2021/image-20220407212250046.png" alt="image-20220407212250046" style="zoom:50%;" />

# 前言:

大家好，我是 LINE 台灣的資深技術推廣工程師的 Evan Lin 。 



# 解決的問題痛點

# Demo (展示) :

# 開發流程記錄：

這邊透過一些開發流程，來跟各位分享該如何在 LINE 聊天機器人中來完成這樣的開發：

## 如何在 Heroku 上面開發 Golang 的 App 

![image-20220412143245984](../images/2021/image-20220412143245984.png)

以往的時候，如果要使用 Google Cloud 相關的 API (比如說: Google Cloud Storage) 存取 API ，只能透過 JSON 檔案來操作。但是如果你想要放上 Heroku 的時候，就必須要放上 GitHub ，這個時候就很容易不小心誤放 JSON 檔案而被機器人掃走而盜用。透過環境變數跟 Golang Buildpack 可以幫助你在安全無慮的狀況下部署 Herokuu 專案，也可以開源到 Github ，歡迎[參考這篇文章](https://www.evanlin.com/til-heroku-gcp-key/)。

以下也提供一個簡單的測試程式碼，可以確認連接到 GCP 是否有正確連結。

<script src="https://gist.github.com/kkdai/1a81605d33f22603762354e981990596.js"></script>

## 如何投過官方帳號 (Official Account) 來抓取 LINE 聊天對話中的影片或是聲音檔案

首先，當官方帳號收到圖片訊息(image)，影片訊息(video) 或是聲音訊息(audio) 的時候，通常會透過以下幾個 Message Hook.

- [Image Messagge Webhook](https://developers.line.biz/en/reference/messaging-api/#wh-image)
- [Video Message Webhook](https://developers.line.biz/en/reference/messaging-api/#message-event)
- [Audio Messageg Webhook](https://developers.line.biz/en/reference/messaging-api/#message-event)

以 Video Message Webhook 為例子，你會看到以下的例子（來自 LINE Dev Doc)

<img src="../images/2021/image-20220411171226450.png" alt="image-20220411171226450" style="zoom:50%;" />

- 如果 Video Message 的影片提供者是外部服務，可能還可以取得檔案的連結。
- 但是如果你透過上傳影片給官方帳號，你可能會發現官方帳號無法直接存取到檔案內容。需要透過  [Get content](https://developers.line.biz/en/reference/messaging-api/#get-content) 的 API 來取的相關資訊。

以下提供範例的 Golang 程式碼，來展示如何取得上傳到 LINE 平台的影片檔案內容：

```go
	for _, event := range events {
		if event.Type == linebot.EventTypeMessage {
			switch message := event.Message.(type) {
			.......
      case *linebot.VideoMessage:
              //Get Video binary from LINE server based on message ID.
              content, err := bot.GetMessageContent(message.ID).Do()
              if err != nil {
                log.Println("Got GetMessageContent err:", err)
              }
              defer content.Content.Close()
```

這個方式就可以取得檔案的 IO Reader 內容，就可以透過 `content` 這個 IO Reader 來將檔案抓下來，或是直接上傳到 Google Cloud Storage 。

## 如何將檔案上傳到 Google Cloud Storage

<script src="https://gist.github.com/kkdai/9b8a82ccdf1294d8a7ae5f7999fd42f4.js"></script>



先看程式碼，以下幾件事情要注意：

- 需要有 cancel 的 context 避免上傳時間過久，造成 API Error 。
- 依照前一個檔案範例，其實可以直接拿 `bot.GetMessageContent(message.ID).Do()` 裡面的 `content` IO Reader 來使用。不需要真的下載到本地端後，再上傳到 GCS 。
- 上傳到 GCS 的檔名，建議不要重複了。 可以使用以下方式來產生唯一的名字。 

```go
func buildFileName() string {
	return time.Now().Format("20060102150405")
}
```

上傳到 GCS 之後，就可以進入下一個階段，將檔案送到 STT 服務 - Google Speech-To-Text 來判斷。

## 如何將讓 STT 服務來判斷檔案裡面的文字

### 透過 STT console 線上測試你的檔案

<img src="../images/2021/image-20220413163042927.png" alt="image-20220413163042927" style="zoom:33%;" />

這裡以 [Google Speech To Text](https://cloud.google.com/speech-to-text?hl=zh-tw) 為例子，先到控制台打開相關服務後到 [Speech Console](https://console.cloud.google.com/speech) 。並且可以在控制台上直接上傳測試一段小影片，這邊有幾個參數可以解釋一下：

- 檔案選擇上，可以 `選擇 MP4 來作為 STT 的判斷檔案，只要 Audio Encoding 選擇正確就好` 。 
- **Encoding** :記得選 **MP3**， 不然通常都會有問題。 iPhone  手機都是 MP4 container 加上 MP3 Audio Encoding 。
- **Sample Rate** : 記得選 48000 

<img src="../images/2021/image-20220413163735523.png" alt="image-20220413163735523" style="zoom:33%;" />



下一個項目也有一些可以調整：

- **Spoken language**: 記得選 zh-TW ，除非你講英文錄影。
- **Transcription model**:選擇  Default 其實就很夠用。

以上流程大概就是一個讓你透過 Console 來測試你的音訊檔案，這邊有幾件事情可以注意到。

- Google STT 可以直接吃影片檔案，不用自己跑音訊擷取。
- Google STT 可以直接吃影片檔案，不用自己跑音訊擷取。
- Google STT 可以直接吃影片檔案，不用自己跑音訊擷取。

並且要注意你的 Sample Rate 不要挑選錯誤。才能讓聲音正確被讀取到。

## 使用 Go GCP STT SDK 來判斷





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

已經修改資料夾權限，讓他可以 Public 後。如何取得 Google Cloud Storage 對外鏈結呢？

規則如下：

```
https://storage.googleapis.com/{{BucketName}}/{{ObjectName}}
```

所以可以寫成一個簡單的 function ，來幫你產生。








# 相關文章：

- [GCS Doc: Make data public](https://cloud.google.com/storage/docs/access-control/making-data-public)
- [iThome 鐵人賽 - Day 23 Google Cloud Speech-to-Text - 子系列最終章](https://ithelp.ithome.com.tw/articles/10223077?sc=iThelpR)
- [[學習文件\] 如何在 Heroku 上面使用透過 Golang 來存取 Google Cloud 服務](http://www.evanlin.com/til-heroku-gcp-key/)
- [GCP: Speech-to-Text client libraries](https://cloud.google.com/speech-to-text/docs/libraries?hl=zh-tw#client-libraries-resources-go)
-  [Stackoverflow: How do I retrieve a file from GCS using a browser if all I know is the gs scheme URI?](https://stackoverflow.com/questions/34214491/how-do-i-retrieve-a-file-from-gcs-using-a-browser-if-all-i-know-is-the-gs-scheme)

  

   
