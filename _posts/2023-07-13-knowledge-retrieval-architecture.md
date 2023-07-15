---
layout: post
title: "[TIL] Knowledge Retrieval Architecture for LLM’s (2023)筆記"
description: ""
category: 
- TodayILearn
tags: ["GPAI", "TIL", "LLM"]
---

![Knowledge Retrieval Architecture for LLM’s (2023)](../images/2022/featured-image-llm-knowledge-architecture.png)

在網路上尋找關於 RAG 跟 LangChain 相關的文章，偶然發現這篇好文： [Knowledge Retrieval Architecture for LLM’s (2023)](https://mattboegner.com/knowledge-retrieval-architecture-for-llms/)



# 摘要：

這一篇專門在講怎麼做一個 LLM Retrieval Architecture (中文不太會翻 XD)
從最簡單的 [OpenAI Cookbook repo](https://github.com/openai/openai-cookbook/blob/main/examples/Question_answering_using_embeddings.ipynb?ref=mattboegner.com) 到， RAG, RETRO 到 REALM 架構都有提到。

也有提出之前資料假象的問題，找了兩篇論文來討論 [HyDE](https://arxiv.org/abs/2212.10496?ref=mattboegner.com) 跟 [GenREAD](https://arxiv.org/abs/2209.10063?ref=mattboegner.com) 。

# 詳細解釋

## 查詢大型資料的問題：

- 要小心 4000 token 的限制

- Token 越多，處理速度越慢。

## 解決方式

![img](../images/2022/Screen-Shot-2023-01-31-at-12.44.17-AM-20230714162721032.png)

-  **(R: Retrieval)**（上圖）透過外部資料的導入
  - 許多外部資料， txt/pdf/API 透過切開來後透過 Embedding 放入某些向量空間的 Embedding Store 之中。


![img](../images/2022/Screen-Shot-2023-01-31-at-12.44.42-AM.png)

- （上圖）使用者問的問題，也透過 Embedding 來找到相關資料。 比如說： 問哪裡可以找到鍛鍊跟減肥的場地，就會跟健身房場地的「向量空間」相當的接近。
- **(A: Augment)** 增強：
  -  將相關內容，跟使用者的問題，一起放在 Prompt 中去詢問。但是為了讓提問的答案效果會更好，可能需要有相關增強的相關語句。比如說：
    - `請用中文回覆`
    - `請不要回覆你不知道的答案`
    - `必要時候，請給我追問語句`
  - 相關資料可以看： [OpenAI Cookbook repo](https://github.com/openai/openai-cookbook/blob/main/examples/Question_answering_using_embeddings.ipynb?ref=mattboegner.com) 
- **G: Generative) 生成:**
  - 就是把相關資料丟入 LLM 去生成解答。



# 優化設計

有一些方式可以優化以上的基本架構：

## 產生更有意義的 Retrieval 資料

- [Precise Zero-Shot Dense Retrieval without Relevance Labels: (a.k.a. HyDE:  Hypothetical Document Embeddings~(HyDE).)](https://arxiv.org/abs/2212.10496?ref=mattboegner.com) 
  - 論文摘要:
    - 本文提出了一種通過虛擬文件嵌入來解決零樣本情況下的密集檢索系統的困難。在沒有相關性標籤的情況下，HyDE通過指示模型生成虛擬文件，該文件捕捉了相關性模式，但仍然不真實且可能包含錯誤細節。然後，通過無監督對比學習的編碼器將該文件編碼為嵌入向量。該向量在嵌入空間中識別出一個近似的鄰域，根據向量相似性檢索出相似的真實文件。我們的實驗結果表明，HyDE明顯優於最先進的無監督密集檢索器Contriever，在各種任務（例如網絡搜索、問答、事實驗證）和語言（例如瑞典語、韓語、日語）上表現出強大的性能。
  - 透過 LLM 來增加文章本體的上下文，透過這樣的方式來增加整體回覆的完整性。也能進而提升回覆的正確性。
- [名為“生成然後讀取”(GenRead): Generate rather than Retrieve: Large Language Models are Strong Context Generators](https://arxiv.org/abs/2209.10063?ref=mattboegner.com) 
  - 論文摘要：
    - 本論文提出了一種解決知識密集型任務的新方法，該方法使用大型語言模型生成文檔，而不是使用文檔檢索器。這種方法首先給語言模型提供問題，生成相關的文檔，然後閱讀這些文檔以生成最終答案。此外，作者還提出了一種基於聚類的提示方法，從而選擇不同的提示，生成涵蓋不同觀點的文檔，從而提高答案的回收率。研究人員在三個不同的知識密集型任務上進行了廣泛實驗，包括開放領域問答、事實檢查和對話系統。其中，該方法在TriviaQA和WebQ上取得了71.6和54.4的精確匹配分數，明顯優於目前最先進的檢索-閱讀流程DPR-FiD的+4.0和+3.9，而且不需要從任何外部知識來源檢索文檔。最後，作者還演示了通過結合檢索和生成可以進一步提高模型性能。相關代碼和生成的文檔可以在https://github.com/wyu97/GenRead 找到。
  - 也是類似 HyDE 的方式，而是採取多個上下文的方式來確保內容更有意義。



# 相關資料

-  [論文：HyDE](https://arxiv.org/abs/2212.10496?ref=mattboegner.com) 
- [論文：GenREAD](https://arxiv.org/abs/2209.10063?ref=mattboegner.com) 。
-  [OpenAI Cookbook repo](https://github.com/openai/openai-cookbook/blob/main/examples/Question_answering_using_embeddings.ipynb?ref=mattboegner.com) 
- [GPT指數](https://github.com/jerryjliu/gpt_index?ref=mattboegner.com)
- 用於語義搜索和其他 NLP 應用的[Haystack庫](https://docs.haystack.deepset.ai/docs/intro?ref=mattboegner.com)
- [知識密集型 NLP 任務的檢索增強生成](https://arxiv.org/abs/2005.11401?ref=mattboegner.com)
- [浪鏈](https://github.com/hwchase17/langchain?ref=mattboegner.com)
- [如何使用 GPT3、嵌入和數據集對文檔實施問答](https://simonwillison.net/2023/Jan/13/semantic-search-answers/?ref=mattboegner.com)
- [FAISS](https://github.com/facebookresearch/faiss?ref=mattboegner.com)用於向量相似度計算
- [生成而不是檢索：大型語言模型是強大的上下文生成器](https://arxiv.org/abs/2209.10063?ref=mattboegner.com)
- [GenRead 範例程式碼](https://github.com/wyu97/GenRead)
