---
layout: post
title: "[Gemma] 用 Gemma/Gemma2 這類 Local Model 打造更安全與具有隱私概念的 LINE ChatBot"
description: ""
category: 
- Python 
- TIL
tags: ["python", "gemma", "GoogleCloud"]
---

<img src="../images/2022/OIG1.MhE3CMXSMoTMNyqkN-20240721175659088.jpeg" alt="卡通風格，一個聊天機器人同時握手 Alpaca  跟 大企業穿著西裝的上班族 。三個人一起握手" style="zoom:50%;" />

## 前言

[Google Gemma2/PaliGemma\] Gemma2/PaliGemma 學習筆記，可以應用範圍](http://www.evanlin.com/google-gemma2_study_note/) 這篇文章中我們有稍微介紹過如何在 LINE Bot 中如何使用 Gemma 這種稱為可以建置在本地端的 Local Model 。

本篇文章將更仔細的解釋相關用法，並且提供一些範例程式碼作為 LINE Bot 的範例。



## Gemma/LLAMA 這一類的模型該如何部署

不論是 Gemma 還是 LLAMA 這一類可以部署在本地電腦（或是自己的雲端伺服器裡面的），在本文中都先暫且稱為 Local Model 。 他的基本 Prediction 的精準度，在於你提供的本地機器的算力。

### 筆電使用上 可以考慮使用 Ollama

![ollama-webui · GitHub](../images/2022/147204191.png)

Ollama 是一個跨平台很好使用 LLM 的本地端工具，可以在本地端的電腦去使用 Llama3, Phi 3, Mistral, Gemma2 等等本地端的模型。使用跟安裝也相當簡單，基本上現在只要是 M1 或是 M2 的 Mac Book 就可以很輕鬆地跑起相關的服務。



### GCP / Vertex AI 上面要部署這些模型

<img src="../images/2022/intro-billboard-v2.png" alt="img" style="zoom:67%;" />

可以透過 Vertex AI 的服務來部署

- [Gemma](https://console.cloud.google.com/vertex-ai/publishers/google/model-garden/335)
- [Gemma2](https://console.cloud.google.com/vertex-ai/publishers/google/model-garden/gemma2)
- [PaliGemma](https://console.cloud.google.com/vertex-ai/publishers/google/model-garden/paligemma)

但是需要申請伺服器單位如列表 

| Gemma / Gemma2                                               | PaliGemma                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Machine type: ct5lp-hightpu-4t Accelerator <br />type: TPU_V5_LITEPOD <br />Accelerator count: 4 | - Machine type: g2-standard-16<br/>- Accelerator type: NVIDIA_L4<br/>- Accelerator count: 1 |


不過要注意這些單位需要申請，因為筆者還沒有申請下來本文將使用 Replicate 來示範。 （2024/07/19)



### 透過第三方模型託管服務 Replicate 

<img src="../images/2022/Google Chrome 2024-07-21 17.52.04.png" alt="Google Chrome 2024-07-21 17.52.04" style="zoom:25%;" />

Replicate AI 是一間可以在雲端去測試這些本地端 Model 的網路服務提供商，可以在雲端上透過 UI 快速去了解並且測試。也可以 fint-tune 與部署自己本地端的模型在他們的服務上面。 本文的範例將這過他們的服務來架設。



## 如何透過 Replicate AI 來執行 Gemma 或是 Gemma2

這邊可以直接去尋找 Replicate AI 上面的資料，可以找到以下相關資料：

- [google-deepmind/gemma-7b-it](https://replicate.com/google-deepmind/gemma-7b-it)
- [lucataco/gemma2-9b-it](https://replicate.com/lucataco/gemma2-9b-it)

### 相關設定也相當簡單（以 Python 為範例)：

1. 設定環境變數 `REPLICATE_API_TOKEN` 

```shell
export REPLICATE_API_TOKEN=r8_d8o**********************************
```



2. 安裝套件

```shell
pip install replicate
```



3. 直接執行以下的程式碼

```python
import replicate

output = replicate.run(
    "lucataco/gemma2-9b-it:24464993111a1b52b2ebcb2a88c76090a705950644dca3a3955ee40d80909f2d",
    input={
        "top_k": 50,
        "top_p": 0.9,
        "prompt": "Write me a poem about Machine Learning.",
        "temperature": 0.6,
        "max_new_tokens": 512,
        "repetition_penalty": 1.2
    }
)

# The lucataco/gemma2-9b-it model can stream output as it's running.
# The predict method returns an iterator, and you can iterate over that output.
for item in output:
    # https://replicate.com/lucataco/gemma2-9b-it/api#output-schema
    print(item, end="")
```



## 如何跟 LINE Chatbot 可以有完美的結合？

Gemma 與 LLAMA 這種本地端模型有哪些優勢？
