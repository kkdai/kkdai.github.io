---
layout: post
title: "[Golang][Gemini Pro] 使用 Gemini-Pro-Vision 來打造名片管理的聊天機器人"
description: ""
category: 
- Golang
tags: ["Golang", "GoogleGemini", "LLM"]

---

![img](../images/2022/add_card.jpg)



# 前提

在之前的文章中，探討了如何使用 Golang 結合 Google Gemini Pro 來開發一個具備大型語言模型（LLM）功能的 LINE Bot。這些文章分別介紹了如何整合 Gemini Pro 的聊天完成（Chat Completion）和圖像識別（Image Vision）功能：

1. [使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (一）: 聊天完成與圖像識別](https://www.evanlin.com/til-gogle-gemini-pro-linebot/)
2. [使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (二）: 使用聊天會話（Chat Session）與 LINE Bot 快速整合，打造具有記憶功能的 LINE Bot](https://www.evanlin.com/til-gogle-gemini-pro-chat-session/)

這次，將簡要介紹如何利用 Gemini Pro Vision 模型來創建一個能夠幫助你整理名片的小工具，它甚至能自行識別名片上的資訊。

##### 相關開源程式碼：

#### [https://github.com/kkdai/linebot-smart-namecard](https://github.com/kkdai/linebot-smart-namecard)

註解： 關於如何使用 Notion 作為線上免費的資料庫，請參考這篇文章 : [[Golang\][Notion] 如何透過 Golang 來操控 Notion DB 當成線上資料庫](https://www.evanlin.com/til-golang-notion-db/) 。

## 系列文章：

1. [使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (一）: Chat Completion and Image Vision](https://www.evanlin.com/til-gogle-gemini-pro-linebot/)
2. [使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (二）: 使用 Chat Session 與 LINEBot 快速整合出有記憶的 LINE Bot ](https://www.evanlin.com/til-gogle-gemini-pro-chat-session/)
3. 使用 Golang 透過 Google Gemini Pro 來打造一個具有LLM 功能 LINE Bot (三）: 使用 Gemini-Pro-Vision 來打造名片管理的聊天機器人 (本篇)



# 辨識名片處理上的小訣竅

## 執行的 Prompt 
關於打造一個名片辨識的部分，這裡分享相關的做法：

```
// Const variables of Prompts.
const ImagePrompt = "這是一張名片，你是一個名片秘書。請將以下資訊整理成 json 給我。如果看不出來的，幫我填寫 N/A， 只好 json 就好:  Name, Title, Address, Email, Phone, Company.   其中 Phone 的內容格式為 #886-0123-456-789,1234. 沒有分機就忽略 ,1234"

```

這個名片分成幾個部分來解釋：

### **解析圖片**： 

相關資訊透過照片的上傳。請 Gemini Pro Vision 來分析。

### **產出格式**： 

這裡有說明，希望 LLm 將資訊透過 json 來提供解決。並且透過以下欄位來分開提供。 這裡也說明一下，這樣說明，就會讓 LLM 會自動去看懂名片上的資訊然後分開提供給你相關資訊。

- Name
- Title
- Address
- Email
- Phone
- Company

### **特殊處理**：

透過 Gemini-Pro-Vision 或是其他 GPT-Vision 大模型來處理影像的時候，都需要準備相關的特殊處理。

#### 關於名片上電話的處理案例：

以下分享幾個電話例子：

- (02) 1234-5678
- (02) 1234 5678
- (02) 1234-5678 轉 123
- (02) 1234-5678 分機 123

根據電話資訊的正確輸入法： 電話好買 *886-02-1234-5678, 1234 (如果分機是 1234) 的話。 使用以下的 Prompt 可以有效的取得:

```
其中 Phone 的內容格式為 #886-0123-456-789,1234. 沒有分機就忽略 ,1234。
```


#### 關於名片上空白數值：

如果在 Notion 資料庫中有著空的數值不會有任何問題。但是如果要使用 Flex Message 卡片格式來放資料。每一個欄位則必須要有數值，不然 LINE 平台無法接受這樣的 Flex Message. 。

而相信許多人也收過，有些人的名片是比較精簡的版本，上面並不會有職稱，或是說不會有電話（比較常見）。這個時候需要填入一些數值，讓資料不會有空值。 相關的 Prompt 為：

```
如果看不出來的，幫我填寫 N/A
```

## GPT Vision 辨識處理 Golang 程式碼

關於 Gemini Pro 影像辨識的程式碼是跟之前（第一篇文章）一樣，這邊就不重新敘述。也可以直接參考 [github](https://github.com/kkdai/linebot-smart-namecard/blob/main/gemini.go) 。 但是這裡寫一下處理的方式：

### 透過外部參數或是環境變數處理 Prompt

在寫 LLM 相關應用的時候，要記得 Prompt 是會隨時去調整來取得最佳的辨識效果。這時候如果 Prompt 是寫在程式碼裡面，就會發生不斷修改與部署。建議要寫在外部資料庫，或是系統環境變數。以下提供相關流程：

```
// Const variables of Prompts.
const ImagePrompt = "這是一張名片，你是一個名片秘書。請將以下資訊整理成 json 給我。如果看不出來的，幫我填寫 N/A， 只好 json 就好:  Name, Title, Address, Email, Phone, Company.   其中 Phone 的內容格式為 #886-0123-456-789,1234. 沒有分機就忽略 ,1234"


// 檢查是否有環境變數，如果沒有，就使用自定義的 Prompt 。
card_prompt := os.Getenv("CARD_PROMPT")
	if card_prompt == "" {
		card_prompt = ImagePrompt
	}
	

// 透過圖片下 Prompt
// Chat with Image
				ret, err := GeminiImage(data, card_prompt)
				if err != nil {
					ret = "無法辨識圖片內容文字，請重新輸入:" + err.Error()
					if err := replyText(e.ReplyToken, ret); err != nil {
						log.Print(err)
					}
					continue
				}
```

### 輸入卡片到資料庫的基本處理

雖然本篇文章不會詳細敘述關於 [Notion 資料庫](https://www.evanlin.com/til-golang-notion-db)的處理。但是這邊稍微提供卡片資料庫的基本處理流程。

- 掃描到卡片後，透過 **Email** 作為卡片的唯一資料來檢查是否有重複資料。
- 如果有 **Email** 相同，則會 skip 本次的掃描資訊。

這部分的處理就算是一個段落，接下來要講解如何透過關鍵字搜尋的相關處理。



# 方便的名片搜尋

以往在搜尋名片的時候，經常會使用一些關鍵字來搜尋。比如說：

- 想要找出所有認識的「經理」
- 想要找位於某間公司的所有窗口
- 想要找出所有認識的行銷窗口
- 印象中認識一位「李教授」但是不確定是哪間學校。

以上的方式都是名片搜尋需要的功能，本段將介紹該如何實作這一段的部分：

## 名片搜尋方式:

這邊稍微列出在 Notion 上面使用的程式碼：

```
				//using test as keyword to query database
				nDB := &NotionDB{
					DatabaseID: os.Getenv("NOTION_DB_PAGEID"),
					Token:      os.Getenv("NOTION_INTEGRATION_TOKEN"),
					UID:        uID,
				}

				// Query the database with the provided uID and text
				results, err := nDB.QueryDatabaseContains(message.Text)
				log.Println("Got results:", results)

				// If there's an error or no results, reply with an error message
				if err != nil || len(results) == 0 {
					ret := "查不到資料，請重新輸入"
					if err != nil {
						ret = fmt.Sprintf("%s: %s", ret, err.Error())
					}
					if err := replyText(e.ReplyToken, ret); err != nil {
						log.Print(err)
					}
					continue
				}

```

其中 QueryDatabaseContains 這個 function 會先尋找 Name, Title 與公司名稱。依照這三個順序，將所有的資料搜尋出來。



# 成果與未來展望

![img](../images/2022/query.jpg)

## 使用方式：

- **新增名片：** 直接透過相片，掃描名片即可。 不需要像其他名片軟體需要抓名片的四周，也不需要等待對應。
- **查詢名片**： 



# 參考資料：

- [OpenAI ChatCompletion API](https://platform.openai.com/docs/guides/text-generation/chat-completions-api)
- [google.generativeai.ChatSession](https://ai.google.dev/api/python/google/generativeai/ChatSession?hl=en)
- [Google AI Studio API Price](https://ai.google.dev/pricing)
- [GoDoc ChatSession Example](https://pkg.go.dev/github.com/google/generative-ai-go/genai#example-ChatSession)
- [Google GenerativeAI ChatSession Python Client](https://ai.google.dev/api/python/google/generativeai/ChatSession?hl=en) 
-  [[Golang\][Notion] 如何透過 Golang 來操控 Notion DB 當成線上資料庫](https://www.evanlin.com/til-golang-notion-db/) 
