---
layout: post
title: "[DeepLearning] Finetuning Large Language Models - 整理"
description: ""
category: 
- Coursera
tags: ["Python"]

---

![image-20230826213922965](../images/2022/image-20230826213922965.png)



## 起因

OpenAI 3.5 發布 Fine-Tuning API 後，另外一邊 Andrew Ng 馬上上線相關課程：
[https://deeplearning.ai/short-courses/finetuning-large-language-models/](https://deeplearning.ai/short-courses/finetuning-large-language-models/)

## Why Finetune

附上大家容易有疑慮的比較圖：為何弄 Fine-Tune 為何不透過 Vector Prompting

![Image](../images/2022/F4V2Xqra0AAGY7b.jpeg)



![image-20230826220610545](../images/2022/image-20230826220610545.png)

- 確認原本 LLM 不能完成
- 尋找 gold samples
- 確認可以比較好結果
- Finetune it.



