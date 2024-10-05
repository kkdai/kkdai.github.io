---
layout: post
title: "[Google Cloud/Firebase] 如何在 GCP Cloud Run 上面透過 LangChain 取得 YouTube 的相關資訊 "
description: ""
category: 
- Python 
- TIL
tags: ["python", "LangChain", "GoogleCloud"]
---



## 前提





透過以下的方式可以在 [Google Cloud 啟動 YouTube API](https://console.developers.google.com/apis/api/youtube.googleapis.com/overview?project=660825558664)



# 呼叫 LangChain YouTube Downloader 來取得影片字幕

參考 LangChain 教學文件 ([YouTube transcripts](https://python.langchain.com/docs/integrations/document_loaders/youtube_transcript/)) ，透過這個套件可以取得具有字幕的 YouTube 影片。透過相關的分析可以快速了解影片內容。這邊有一些範例程式碼：

```python
%pip install --upgrade --quiet  pytube
```



```python
loader = YoutubeLoader.from_youtube_url(
    "https://www.youtube.com/watch?v=QsYGlZkevEg", add_video_info=True
)
loader.load()
```



# 在 GCP CloudRun 上面卻有完全不一樣的結果

嘗試著部署到 Google CloudRun

# 以往透過 LangChain 
