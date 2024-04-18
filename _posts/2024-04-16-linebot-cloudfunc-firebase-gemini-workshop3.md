---
layout: post
title: "[BwAI workshop][Golang] LINE OA + CloudFunction + GeminiPro + Firebase = 旅行小幫手 LINE 聊天機器人(3)： 導入「名片小幫手」跟「收據小幫手」"
description: ""
category: 
- Python 
- TIL
tags: ["Golang", "LINEBot", "Firebase", "GoogleCloud", "CloudFunction"]
---

![image-20240418150533579](../images/2022/image-20240418150533579.png)



# 前言:

這是一篇為了 04/18 跟 Google Developer Group 合作的 BUILD WITH AI (BWAI) WORKSHOP 的最後一篇文章，畢竟晚上就要工作坊了（投影片可以當場弄，文章可來不及當場寫 XD ）。

請記得，如果你想知道以下相關知識：

- [如何申請 LINE Developer 帳戶，如何建立一個 LINE OA (ChatBot) Channel？](https://www.evanlin.com/linebot-cloudfunc-firebase-gemini-workshop/) （範例： [辨識圖片 LINEBot](https://github.com/kkdai/linebot-cloudfunc-gemini-go))
- [如何透過 Google Cloud Functions 使用 Google Credential 來操作 Firebase Realtime Database](https://www.evanlin.com/linebot-cloudfunc-firebase-gemini-workshop2/) (範例： [具有長記憶的聊天機器人](https://github.com/kkdai/linebot-cf-firebase))

本篇文章將專注在以下幾個部分：

- 將兩個範例程式 [名片小幫手（舊版用 notion）](https://github.com/kkdai/linebot-smart-namecard)與[收據小幫手（Python 舊版)](https://github.com/kkdai/linebot-receipt-gemini) ，改寫成 Golang 。
- 分享 Golang 在操作 Firebase Realtime Database 資料庫上幾個需要注意的地方。
- 最後分享做 Gemini-Vision 的一些心得與未來可以改善的空間。



# 文章列表：

-  [[BwAI workshop][Golang] LINE OA + CloudFunction + GeminiPro + Firebase = 旅行小幫手 LINE 聊天機器人(1)： 景色辨識小幫手](https://www.evanlin.com/linebot-cloudfunc-firebase-gemini-workshop/)
-  [[BwAI workshop][Golang] LINE OA + CloudFunction + GeminiPro + Firebase = 旅行小幫手 LINE 聊天機器人(2)： Firebase Database 讓 LINEBot 有個超長記憶](https://www.evanlin.com/linebot-cloudfunc-firebase-gemini-workshop2/)

### 程式碼列表：

- [名片小幫手（舊版用 Golang + Notion）](https://github.com/kkdai/linebot-smart-namecard)
- [收據小幫手（Python 舊版)](https://github.com/kkdai/linebot-receipt-gemini) 
- [辨識圖片 LINEBot](https://github.com/kkdai/linebot-cloudfunc-gemini-go)
- [具有長記憶的聊天機器人](https://github.com/kkdai/linebot-cf-firebase)
- [名片小幫手（新版： Golang + Firebase RealtimeDB + Cloud Functions)](https://github.com/kkdai/linebot-cf-namecard)
- [收據小幫手（新版： Python -> Golang + Firebase DB + Cloud Functions)](https://github.com/kkdai/linebot-cf-receipt)




# 事前準備

- **[LINE Developer Account](https://developers.line.biz/en/)**: 你只需要有 LINE 帳號就可以申請開發者帳號。
- [**Google Cloud Functions**](https://cloud.google.com/functions?hl=zh_cn)： ＧGo 程式碼的**部署平台**，生成供 LINEBot 使用的 webhook address。
- [**Firebase**](https://firebase.google.com/)：建立**Realtime database**，LINE Bot 可以記得你之前的對話，甚至可以回答許多有趣的問題。
- **[Google AI Studio](https://aistudio.google.com/)**:可以透過這裡取得 Gemini Key 。

務必確定已經有前兩篇文章的環境，並且該 Cloud Functions 已經串接好 LINE Bot 。

![image-20240418144300796](../images/2022/image-20240418144300796.png)



# 名片小幫手的導入：

- 將程式碼: https://github.com/kkdai/linebot-cf-namecard 裡面的 `function.go` 打開。
- 複製到 Cloud Functions 中已經設置好的環境。
- 記得在 Firebase Realtime Database 建立一個新的 set - `namecard`

![image-20240418144648316](../images/2022/image-20240418144648316.png)

- **部署 (搭啦)**



## 名片小幫手程式碼修改部分

首先，將原本透過 Notion 作為 DB 改成 Firebase Realtime Database 之後。首先需要定義相關資料結構。

```
// Person 定義了 JSON 資料的結構體
type Person struct {
	Name    string `json:"name"`
	Title   string `json:"title"`
	Address string `json:"address"`
	Email   string `json:"email"`
	Phone   string `json:"phone"`
	Company string `json:"company"`
}
```

資料寫入的部分，這邊使用到 [Firebase Database 中的 Push](https://firebase.google.com/docs/database/admin/save-data#go) 。相關的官方說明如下：

```
Add to a list of data in the database. Every time you push a new node onto a list, your database generates a unique key, like messages/users/<unique-user-id>/<username>
```

也就是說，資料是用類似以下方式儲存：

![image-20240418145754861](../images/2022/image-20240418145754861.png)

儲存之前很簡單，不需要額外資料轉換。只需要直接 `Push` 進去，前面會加上一個唯一的 key 值。

```
				const DBCardPath = "namecard"
				.....
				
				// Insert the person data into firebase
				userPath := fmt.Sprintf("%s/%s", DBCardPath, uID)
				_, err = fireDB.NewRef(userPath).Push(ctx, person)
				if err != nil {
					log.Println("Error inserting data into firebase:", err)
				}
```

但是在前面取用的時候，就會比較複雜。因為要完整把整包 JSON 都抓下來後處理。需要以下變動。

```
				// Load all cards from firebase
				var People map[string]Person
				err = fireDB.NewRef(userPath).Get(ctx, &People)
				if err != nil {
					fmt.Println("load memory failed, ", err)
				}

				// Marshall data to JSON
				jsonData, err := json.Marshal(People)
				if err != nil {
					fmt.Println("Error marshalling data to JSON:", err)
				}

```

透過一個 string key map  `var People map[string]Person` 來處理這樣的資料格式，再來直接轉換成 JSON 。這樣又可以將資料完整抓下來變成 Gemini 可以閱讀的方式來處理。

## 名片小幫手改進後相關成果

![image-20240418141427354](../images/2022/image-20240418141427354.png)

#### 程式碼： **名片小幫手(Go Cloud Functions版本)**：[https://github.com/kkdai/linebot-cf-namecard](https://github.com/kkdai/linebot-cf-namecard)

這裡備註一些主要修改，跟優化的部分：

- 由於使用 Firebase Database ，整包 JSON 丟給 LLM 變得更加聰明。



# 旅遊小幫手導入

- 將程式碼: [https://github.com/kkdai/linebot-cf-receipt]( https://github.com/kkdai/linebot-cf-receipt ) 裡面的 `function.go` 打開。
- 複製到 Cloud Functions 中已經設置好的環境。
- 建立一個 Firebase Database set: `receipt` 
- **部署 (搭啦)**

## 相關修改部分

- 



# 總結：





#  完整原始碼

你可以在這裡找到相關的開源程式碼: 

- **名片小幫手(Go Cloud Functions版本)**：[https://github.com/kkdai/linebot-cf-namecard](https://github.com/kkdai/linebot-cf-namecard)
- **收據小幫手(Go Cloud Functions版本)**：[https://github.com/kkdai/linebot-cf-receipt](https://github.com/kkdai/linebot-cf-receipt)



