---
layout: post
title: "[Golang] 如何使用 Golang 透過 LangChain 架構來使用 LLAMA"
description: ""
category: 
- Golang
tags: ["LangChain", "LLAMA", "VertexAI"]

---



# 前提

好久沒來寫 Golang 來寫文章了，就想說把之前看過關於 Golang LangChain 相關的文章來敘述一下。以下這篇文章主要參考了 [Eli Bendersky 部落格的文章 - [Using Ollama with LangChainGo](https://eli.thegreenplace.net/2023/using-ollama-with-langchaingo/)](https://eli.thegreenplace.net/2023/using-ollama-with-langchaingo/)

 這裡也會詳細一點來介紹以下幾個部分：

- 什麼是 Ollama 能拿來做些什麼？
- 要如何使用 Ollama ?
- 如何透過 Golang 連接 Ollama 並且串接 LangChain 
- 來個有趣的範例吧



# 什麼是 Ollama

![image-20231211231527061](../images/2022/image-20231211231527061.png)

[Ollama](https://ollama.ai/) 是一個相當方便的工具，以往需要在本地端使用 llama 的話需要有以下的步驟：

- [到 Meta AI 申請下載 link](https://ai.meta.com/llama/)
- 透過 [LLAMA.cpp](https://github.com/ggerganov/llama.cpp) 把 LLAMA2 的 model 去轉換過後，讓你在 Mac OSX 上面可以執行並且讀取。 （當然還有 