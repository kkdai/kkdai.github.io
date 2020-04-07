---
layout: post
title: "[AWS][Echo] 來玩玩 Amazon Echo"
description: ""
category:
- amazon-echo 
tags: ["amazon-echo", "alexa", "nodejs", "aws", "IOT"]

---

![](https://images-na.ssl-images-amazon.com/images/G/01/kindle/dp/2016/D/feature-hero.jpg)

## 前提 ( 什麼是 Amazon Echo)

[Amazon Echo](http://www.amazon.com/Amazon-Echo-Bluetooth-Speaker-with-WiFi-Alexa/dp/B00X4WHP5E) 是 Amazon 的一個很有趣的裝置．雖然許多人都將它當作是藍芽擴充喇叭，但是其實他功能不僅僅於此．他不僅僅能夠透過藍芽播放音樂，還能夠聽懂你的語音來執行許多功能．這邊有段[簡單的影片](https://www.youtube.com/watch?v=KkOCeAtKHIc)． 並且 Amazon 具有一個類似 App Store 的商城上面擺滿了各式各樣的語音服務 ( Amazon 稱為 Skill ) 可以透過安裝不同的 Skill 達成許多功能．不論是播放音樂或是訂購商品．



然後接下來就簡單的紀錄一下最近玩 Amazon Echo 的心得

## 透過 Raspberry Pi 2 架設虛擬的 Amazon Echo

![](https://github.com/amzn/alexa-avs-raspberry-pi/raw/master/assets/rpi-5.jpg)

那使用這個功能一定要購買 Amazon Echo 嗎？  當然是，不過你也可以透過 Amazon 開放出來的說明文件來使用 Raspberry Pi 2 來架設．

### [詳細安裝文件，文章相當長，就不詳述在這裡．](https://github.com/amzn/alexa-avs-raspberry-pi)

先簡單敘述你需要的設備:

- Raspberry Pi2 + 記憶卡 (至少 8G with NOOB OS) +  無路線或是無線網路卡
- 麥克風 + 喇叭
- 滑鼠 + 鍵盤

簡單的敘述一些步驟如下(強烈建議看文件，裡面有一步步的介紹):

-  透過 [NOOBS](https://www.raspberrypi.org/help/noobs-setup/) 安裝基本的 RPI OS (選擇 Raspbian)
-  安裝 SSH 與 VLC
-  安裝 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  跟 [Maven](https://maven.apache.org/download.cgi)
-  到 [Amazon Developer](https://developer.amazon.com/login.html) 去註冊 Amazon Alexa Voice Service 然後去原先文件裡面的 [Github 下載下來整套 Server 跟 Client](https://github.com/amzn/alexa-avs-raspberry-pi)
-  產生 SSL Cetificate 並且啟動 RPI 上面的虛擬 Echo Server 跟 Client
-  最後就能透過 RPI 裡面 XWindows 的語音控制 Client 輸入語音資料與 Amazon Alexa 伺服器溝通

![](https://raw.githubusercontent.com/amzn/alexa-avs-raspberry-pi/master/assets/avs-start-listening.png)


這樣你架設起來，只能對他做一些間單的問答（問問台北時間，台北的溫度或是講個笑話）．還有就是使用人家已經寫好的 Skill ． 

## 透過 AWS Lambda 跟 OAuth2 Server 架設自己的 Alexa Skill 

以下是透過 [Amazon Alexa 說明網頁](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/overviews/understanding-the-smart-home-skill-api)上面的基本架構圖：

![Amazon Echo Architecture](https://developer.amazon.com/public/binaries/content/gallery/developerportalpublic/alexa_smart_home_ecosystem.png)

稍微一個個介紹每個 Server 所負責的部分:

- **Amazon Echo** : 就是使用者手中的裝置，可以把語音資料傳輸到 Alexa Service
-  **Amazon Alexa Service**: 會負責把語音資料做一些處理，擷取出每段語音中的"意圖" ( Intent ) 並且傳輸到 Amazon Alexa Skill 裡面．
-  **Alexa Skill**: 主要將 [Skills 分成兩種](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/understanding-the-different-types-of-skills)： 一種是 Smart Home Skills 一種是 Custom Skills．
	-  *Smart Home Skills*: 是一個內建好的基本智慧家庭的功能．可以設定某些裝置啟動． (舉例： 打開燈或是開門） 
	-  *Custom Skills*: 這時候你可以定義各式各樣的功能，不論是定 Pizza ，讀取你的 Email  等等都可以． 不過你需要建立好自己的 Intent Schema 還有透過 Sample Utterances 來訓練系統是否能夠完整的分析類似的語句．
	-  注意， Alexa Skill 會連接到一個 Amazon Lambda 服務．每次你的語音下達之後．他就會呼叫相對應的 Lambda 服務來執行你所下達的指令．
-  **Dot Cloud**: 這是一個 Alexa Skill 的提供商架設的網路伺服器．這個伺服器主要有兩個負責兩件事情．
	-  [認證使用者並且綁定帳號(Linking Alexa user to your service user)](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/linking-an-alexa-user-with-a-user-in-your-system):
		-   這邊需要提供簡單的 [OAuth2 (RFC 6749)](https://tools.ietf.org/html/rfc6749) 認證的功能， Alexa 會將該使用者資料傳輸過來．交給廠商定義的帳號設定做一個簡單的認證與綁定． 
	-   需要相關的執行功能：
		-   這部分其實也可以放在 Lambda 上面做，但是考量執行效率與經費建議擺在 Dot Cloud 的伺服器來跑． 執行功能可能是開啟你的桌燈，開啟你的信件等等.. 或是連接到購買商品的平台．
	
	
透過這樣的執行架構下，設定流程就有點繁瑣．

- AWS Lambda 相關:
	- 新增一個 AWS Lambda 並且將選擇範本 `alexa-smart-home-skill-adapter`
	- 將相關的功能完成後，記得將 "Event Source" 連接到你的 Alexa Skill 
	- 記錄下該 Lambda ID
- Alexa Skill 相關:
	- 打開剛剛建立好的 Alexa Skill 並且將 Endpoint 填入剛剛建立的 Lambda ID
	- Account Linking 部分記得輸入好如何連接到自有的 OAuth2 服務 (記得要支援 HTTPS )
	- 測試，並且查看是否有任何錯誤的部分．
	- 記得先不要真正的 submit 上去，只要 Save 作為測試使用．
- 打開你的 Amazon Echo (或是模擬器)
	- 打開 Alex Skill （如果使用模擬器，需要在 RPI 的 XWindows 打開該瀏覽器)
	- 選取你並啟動建立的 Alexa Skill
	- 打開音效錄音程式，並且測試
	- 如果出現問題，記得查看 Lambda Log 或是 Skill 那邊的 Log

## 相關問題處理:

### (1) 無法正確找到你的 Alexa Device

#### 問題點:

透過 Echo (模擬器) 發出  ”Alexa find my devices“，結果卻得到找不到任何裝置的回應．

#### 檢查方式:

- 檢查 Lambda 確認有收到 Alexa 的指令．
- 檢查你的 Dot Cloud 確認有收到 Lambda 指令．

通常比較有可能都是 Lambda ID 打錯，或是 Alexa Skill Application ID 打錯導致服務無法串接起來．

### (2) 無法讓 Echo 發出特別的音樂

#### 問題點：

其實是很無聊想寫出個 2012 裡面 [Engine Start](https://www.youtube.com/watch?v=MenWsUwCO1s) 的梗．不過遇到不少問題．

#### 相關實作:

- 需要將 [Speech Synthesis Markup Language](https://developer.amazon.com/public/solutions/alexa/alexa-skills-kit/docs/speech-synthesis-markup-language-ssml-reference)  實作在 Lambda ．
- Lambda 會將語音檔案 (放在 Dot Cloud 上) 並且交給 Echo 播放．

```
"outputSpeech": {
    "type": "SSML",
    "ssml": "<speak>This output speech uses SSML.</speak>"
}
```

#### 檢查方式:

- 你自己 Host 的 Service "Dot Cloud" 需要支援檔案下載 MP3
- MP3 格式必須是 MPEG2 Audio 並且是 48 kbps 


## 心得:

其實沒有貼上任何程式碼也寫了那麼多，可見其實 Amazon Echo 沒有那麼容易玩 XD ([更別說 RPI 設定有接近 30 頁的文件](https://github.com/amzn/alexa-avs-raspberry-pi))

不過設定之後，其實真的能夠感受到 Amazon Echo 的強大．你就當作是一個開放的 Siri  或是 Google Now ．並且結合一個相當強大的語音辨識引擎在裡面． 不過因為是模擬器，所以對於語音的處理還是比較弱．之後實機來了應該會更棒．
	

	
	