---
layout: post
title: "[學習心得][Golang][OpenAI] 關於 OpenAI 新功能: Function Calling"
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



# 在 OpenAI Functions Calling 之前，要怎麼做？

根據 "[DeepLearning 提供一堂很好的 Prompt Engineering for Developers](https://learn.deeplearning.ai/chatgpt-prompt-eng/lesson/1/introduction)" 有提到，你可能要這樣做：

```
你現在是一個協助抓取使用者資訊的小幫手，如果使用者詢問了某個地區的天氣。你就幫我把地區抓出來透過以下格式呈現。 
---
{
   "location": "xxxx"
}
```

<img src="../images/2022/image-20230615123019584.png" alt="image-20230615123019584" style="zoom:25%;" />

很多地方是聰明的，但是還是沒有處理非相關天氣詢問要拒絕。這裡可以透過 [ChatGPT Share](https://chat.openai.com/share/0dbd39c6-246a-4ca4-b941-2a103ec1daf0) 查看結果。如果是一個 NLU 的小幫手，在這個情況下可能要回覆 `{ "location" : ""}` 或是 NULL ，但是這樣會讓你的 Prompt 非常的冗長，而且通常很長的防禦Prompt (咒語)只能防禦一個階段。 那該怎麼辦呢？



# 那 OpenAI Functions Calling 怎麼幫助你呢？

這裡給一張流程圖： [PlantUML](https://www.plantuml.com/plantuml/uml/TP5BIyD058Nt-HM7MIcqMTHTGEq355SML5oMCRbj1kSHvjwXr5_lf6cnXRZAOxxpCUVUEOkEafmjYW-cYEa3LgsMPP0AwZE_mJ2a9Un9vqU4yLW6bk0VLN4Y-z1hHtxnKXt3U4g-5XCyLjfQutSeXkCh-uvaSv9EO4Ej-yJzu2ulrNUnMQnxTPPTfcxK0AlROa2kzCyao1GYSRA2C_pNWp6ReQ5T96AioB99N6RNIAatyirPrWNFX2zTVqF2yQSB3Td-WvDpEfeVAiVwglUnAS8mwXGZUR67RF3-WBsH5Xf2hgEe9KL2s8xUTWArDTvmkucaENXLGMLhTxMQVgyLpFO_5jCChJNpULOIa7AcBEQvTtBs5m00) 

![image-20230615152527422](../images/2022/image-20230615152527422.png)


## 第一步： 呼叫 Chat / Complete Function Calling

 從文章內  "[Function calling](https://openai.com/blog/function-calling-and-other-api-updates)" 可以提供一個很簡單的範例，你可以發現以下得呼叫方式跟原本使用 chat/ completion 沒有差別，但是回傳資訊差很多了。

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

這時候， OpenAI 的 API `絕對` 會回覆給你 JSON ，不需要透過各種黑魔防禦術（特殊 Prompt) 就可以達成。 如果他找得到相關資料，他就會幫你把資料抽取出來，並且放在 `arguments` 傳給你。



更多細節可以[參考 API 文件](https://platform.openai.com/docs/api-reference/chat/create)的部分：

```
Function_call / string or object / Optional

Controls how the model responds to function calls. "none" means the model does not call a function, and responds to the end-user. "auto" means the model can pick between an end-user or calling a function. Specifying a particular function via {"name":\ "my_function"} forces the model to call that function. "none" is the default when no functions are present. "auto" is the default if functions are present.
```



## 跟原本 Prompt 的差別

那你會想說，這樣跟原本透過特殊的 Prompt 有什麼差別？

### **不會有 JSON 以外的回傳：**

如果你這時候，忽然又插嘴一句說：「給我一首詩」，他還是會回傳給你 JSON 檔案。可能會告訴你沒有相關可以使用的 arguments。

### **多個 Functions 的處理：**

這也是最強大的，最好用的地方。請查看

```
curl https: //api.openai.com/v1/chat/completions -u :$OPENAI_API_KEY -H 'Content-Type: application/json' -d '{
"model": "gpt-3.5-turbo-0613",
"messages": [
    {
        "role": "user",
        "content": "What is the weather like in Boston?"
    }
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
                    "enum": [
                        "celsius",
                        "fahrenheit"
                    ]
                }
            },
            "required": [
                "location"
            ]
        }
    },
    {
        "name": "get_current_date",
        "description": "Get the current time in a given location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA"
                },
                "unit": {
                    "type": "string",
                    "enum": [
                        "celsius",
                        "fahrenheit"
                    ]
                }
            },
            "required": [
                "location"
            ]
        }
    }
]
}'
```

你會發現，上面的程式碼有兩個 function 等著判別。 `get_current_date` 跟 `get_current_weather` 。

- 如果你的問句是： `What is the weather like in Boston?` ，那他就會回傳給你 `get_current_weather`，然後給你 `location = Boston`。
- 反之，如果你問句是: `What time is it in Boston?` ，那他就會回傳給你 `get_current_date`，然後給你 `location = Boston`。



## 第二步 : 呼叫 3rd Party API

- 這邊就跳過，可以是各種 Open API 去找資料。
- 但是，可以透過 ChatGPT Plugin 的資料去找，將自己模擬成 ChatGPT Plugin 。

## 第三步： Send the summary to OpenAI
