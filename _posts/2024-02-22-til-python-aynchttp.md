---
layout: post
title: "[LINEBot SDK][Python] 使用 AiohttpAsyncHttpClient 處理 LINE Bot 圖片消息"
description: ""
category: 
- Python
- TIL
tags: ["Python", "LINEBot"]

---

# 使用 AiohttpAsyncHttpClient 處理 LINE Bot 圖片消息

在開發 LINE Bot 應用時，我遇到了一個挑戰：如何有效地處理用戶發送的圖片消息。我需要從 LINE 平台獲取圖片內容，然後將其用於後續的處理。這裡，我想分享我的問題與解決方式，特別是在使用 `AiohttpAsyncHttpClient` 與普通 HTTP 客戶端之間的比較。

套件： [https://github.com/line/line-bot-sdk-python](https://github.com/line/line-bot-sdk-python)

## 問題描述

當用戶通過 LINE Bot 發送圖片時，我需要從 LINE 的服務器上獲取圖片數據。初始的問題出現在嘗試使用一個不存在的 `iter_any` 方法來讀取數據流，這導致了一個 `AttributeError`。這個問題很快被發現並修正，但我還想探索一種更高效的方法來處理這些圖片數據。

## 以前做法

這是以前透過 non-async 的做法，資料會完整讀完才會繼續執行。

```
ef handle_message(event):
	if (event.message.type == "image"):
		SendImage = line_bot_api.get_message_content(event.message.id)

		local_save = './static/' + event.message.id + '.png'
		with open(local_save, 'wb') as fd:
			for chenk in SendImage.iter_content():
				fd.write(chenk)
                
		line_bot_api.reply_message(event.reply_token, ImageSendMessage(original_content_url = ngrok_url + "/static/" + event.message.id + ".png", preview_image_url = ngrok_url + "/static/" + event.message.id + ".png"))
```

這樣有可能會在某些地方卡住很久，造成整個流程無法繼續進行。

## AiohttpAsyncHttpClient 的優勢

`AiohttpAsyncHttpClient` 是 `line-bot-sdk` 的一部分，它提供了一種非同步的方式來處理 HTTP 請求。這與傳統的同步 HTTP 客戶端有著本質的不同。在同步模式下，每個 HTTP 請求都會阻塞當前執行線程，直到收到響應。這在處理大量請求或需要高性能的應用中是不可取的。

相反，`AiohttpAsyncHttpClient` 允許我們發送非同步請求，這意味著我們可以在等待響應的同時繼續執行其他代碼。這對於提高應用的響應性和吞吐量至關重要。

```
from linebot.aiohttp_async_http_client import AiohttpAsyncHttpClient

.....

async_http_client = AiohttpAsyncHttpClient(session)
line_bot_api = AsyncLineBotApi(channel_access_token, async_http_client)
```



## 解決方案

我採用了以下代碼來異步獲取圖片內容：

```python
message_content = await line_bot_api.get_message_content(event.message.id)
image_content = b''
async for s in message_content.iter_content():
    image_content += s
img = PIL.Image.open(BytesIO(image_content))
```

這段代碼使用 `AiohttpAsyncHttpClient` 通過 `line_bot_api.get_message_content` 異步獲取消息內容。然後，我使用 `iter_content` 方法來異步迭代數據塊，並將它們合併到一個二進制字符串中。最後，我使用 `PIL.Image.open` 從這個二進制數據創建了一個圖片對象。

這種方法的好處是，它完全非阻塞。即使在下載大圖片時，我們的應用也可以繼續處理其他事件或消息，從而提高了整體效率。

## 結論

通過使用 `AiohttpAsyncHttpClient`，我成功地解決了處理 LINE Bot 圖片消息的問題，並且提高了應用的性能。這個案例展示了異步編程在現代應用開發中的重要性，特別是在需要處理大量 I/O 操作時。對於開發者來說，理解並利用這些異步工具將是提高應用性能和用戶體驗的關鍵。



## 參考文章：

- [https://ithelp.ithome.com.tw/articles/10280087](https://ithelp.ithome.com.tw/articles/10280087)
- [https://www.myapollo.com.tw/blog/aiohttp-client/](https://www.myapollo.com.tw/blog/aiohttp-client/)

