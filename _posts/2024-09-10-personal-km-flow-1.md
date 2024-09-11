---
layout: post
title: "[Gemini/Firebase] 個人資訊流 - 透過 IFTTT 與 LangChain 打造科技時事 LINE Bot"
description: ""
category: 
- Python 
- TIL
tags: ["python", "firebase", "GoogleCloud", "IFTTT"]
---

![PlantUML diagram](../images/2022/VP312i8m38RlUOgym5vW1ncemy7SPHFFiGlTq5L9MWIVtXQ5IGzU2ltovUSdbNeI7vORaF5tmPEo0AGNYsA3JJqC0vPOipSJA-x84tnW6X_8N5awVkel3DREpjPa6A0bPxSJpIxpO-QPBzWReKUKSs-D-2_sOLb8vXUFgHcMA_XsXSn8IstJxPFARbGyiYfPLgZYDzxX3G00.png)

# 近期

最近弄了一個資訊流，我覺得很有趣：- IFTTT 抓取 HN, HuggingFace 熱門文章

- 發到 Webhook 裡面有 LangChain 抓爬蟲 + LangChain Summary
- 發 LINE Bot 給自己看
- 挑選自己喜歡的，直接貼到 Twitter （目前沒有付費 API 沒辦法發長文

程式碼： [https://github.com/kkdai/gh-summarized-scheduler](https://github.com/kkdai/gh-summarized-scheduler)



# IFTTT 設定

![image-20240911222516788](../images/2022/image-20240911222516788.png)

![image-20240911222612321](../images/2022/image-20240911222612321.png)

# 成果

<img src="../images/2022/image-20240911222313402.png" alt="image-20240911222313402" style="zoom:33%;" />

- 有詳細的文章連結跟文章摘要。
- 可以快速決定要不要進去看。
- 整個格式也改成可以直接複製貼到 Twitter (我有付費發長文)



# 未來發展：

- 照理說應該要可以一鍵發文到 twitter ，但是 X API 好貴（一百美）
- 有想過用 IFTTT 來發文 Twitter 但是有長度限制 (128) ，不能用。
- 不過 Threads API 似乎可以發長文。
