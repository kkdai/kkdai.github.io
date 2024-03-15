---
layout: post
title: "[LINE Bot][Python] Cloud Function + Gemini Pro + Firebase Database == 記憶體聊天機器人"
description: ""
category: 
- Python
- TIL
tags: ["Python", "LINEBot", "Firebase", "GoogleCloud", "CloudFunction"]

---



# 起因

這邊文章，主要是透過[【LineBot實作】如何製作有記憶的對話機器人](https://medium.com/@pearl3904/linebot%E5%AF%A6%E4%BD%9C-%E5%A6%82%E4%BD%95%E8%A3%BD%E4%BD%9C%E6%9C%89%E8%A8%98%E6%86%B6%E7%9A%84%E5%B0%8D%E8%A9%B1%E6%A9%9F%E5%99%A8%E4%BA%BA-0a80a9601e3d) 的相關修改。 把需要付費的服務 OpenAI 改成目前還有免費額度的 Google Gemini ，並且針對相關訊息的程式碼做一些調整。 主要的 LINe Bot 設定與 Firebase 設定請參考原先文章。



# 主要修改

<script src="https://gist.github.com/kkdai/148f57c651f369e771bfd0d86c585563.js"></script>



<script src="https://gist.github.com/kkdai/a59c50a63f568299c46c013461e15d81.js"></script>



## 參考文章：

- [https://ithelp.ithome.com.tw/articles/10280087](https://ithelp.ithome.com.tw/articles/10280087)
- [https://www.myapollo.com.tw/blog/aiohttp-client/](https://www.myapollo.com.tw/blog/aiohttp-client/)

