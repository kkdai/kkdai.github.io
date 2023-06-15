---
layout: post
title: "[學習心得][Golang][OpenAI] 關於 OpenAI 新功能: ChatComplete Func Call"
description: ""
category: 
- 學習文件
- TodayILearn
tags: ["Golang", "ChatGPT", "TIL"]
---

![image-20230615115340699](../images/2022/image-20230615115340699.png)

# 前提

OpenAI 在 06/13 發表了新的功能 "[Function calling](https://openai.com/blog/function-calling-and-other-api-updates)"，其實對 LLM 的開發上算是一個完整的新發展。 本篇文章將快速解釋一下這個更新將會帶來哪一些變革，並且也透過將 LINE 官方帳號的相關整合為案例，幫你打造一個旅遊小幫手。



# 關於 OpenAI 新的功能Function Calling 的細節

關於 "[Function calling](https://openai.com/blog/function-calling-and-other-api-updates)" 主要是拿來處理 Intent 的判斷，並且針對使用者的意圖給予相關的 JSON 輸出給開發者作為處理之用。 換成白話文來說，如果你今天想要做一個「天氣服務的 ChatGPT LINE Bot 」的話，那麼你該如何弄呢？

相關資料：

- 部落格 "[Function calling](https://openai.com/blog/function-calling-and-other-api-updates)" 
- API 文件 [ChatComplete](https://platform.openai.com/docs/api-reference/chat/create)



## 在 OpenAI Functions Calling 之前，要怎麼做？

根據 "[DeepLearning 提供一堂很好的 Prompt Engineering for Developers](https://learn.deeplearning.ai/chatgpt-prompt-eng/lesson/1/introduction)" 有提到，你可能要這樣做：

```
你現在是一個
```



從文章內  "[Function calling](https://openai.com/blog/function-calling-and-other-api-updates)" 可以提供一個很簡單的範例： 

```
curl https://api.openai.com/v1/chat/completions -u :$OPENAI_API_KEY -H 'Content-Type: application/json' -d '{
  "model": "gpt-3.5-turbo-0613",
  "messages": [
    {"role": "user", "content": "What is the weather like in Boston?"}
  ],
  "functions": [
    {
      "name": "get_current_weather",
      "description": "Get the current weather in a given location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "The city and state, e.g. San Francisco, CA"
          },
          "unit": {
            "type": "string",
            "enum": ["celsius", "fahrenheit"]
          }
        },
        "required": ["location"]
      }
    }
  ]
}'
```

也就是你可以透過詢問 `"What is the weather like in Boston?"` 讓 chatGPT 透過 `gpt-3.5-turbo-0613` (這邊要強調是 GPT3.5) 的模型來跑分析，直接回答給你 JSON 檔案。

```
{
  "id": "chatcmpl-123",
  ...
  "choices": [{
    "index": 0,
    "message": {
      "role": "assistant",
      "content": null,
      "function_call": {
        "name": "get_current_weather",
        "arguments": "{ \"location\": \"Boston, MA\"}"
      }
    },
    "finish_reason": "function_call"
  }]
}
```







我們先來看看相關的 API 說明

