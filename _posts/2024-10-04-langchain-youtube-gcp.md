---
layout: post
title: "[Google Cloud/Firebase] 如何在 GCP Cloud Run 上面透過 LangChain 取得 YouTube 的相關資訊 "
description: ""
category: 
- Python 
- TIL
tags: ["python", "LangChain", "GoogleCloud"]
---



# 前提





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



# 在 GCP CloudRun 上面卻拿不到資料

嘗試著部署到 Google CloudRun 之後，這是使用的程式碼：

```
def summarized_from_youtube(youtube_url: str) -> str:
    """
    Summarize a YouTube video using the YoutubeLoader and Google Generative AI model.
    """
    try:
        print("Youtube URL: ", youtube_url)
        # Load the video content using YoutubeLoader
        loader = YoutubeLoader.from_youtube_url(
            youtube_url, add_video_info=True, language=["zh-Hant", "zh-Hans", "ja", "en"])
        docs = loader.load()

        print("Pages of  Docs: ", len(docs))
        # Extract the text content from the loaded documents
        text_content = docs_to_str(docs)
        print("Words: ", len(text_content.split()),
              "First 1000 chars: ", text_content[:1000])

        # Summarize the extracted text
        summary = summarize_text(text_content)

        return summary
    except Exception as e:
        # Log the exception if needed
        print(f"An error occurred: {e}")
        return ""
```

卻得到完全不一樣的成果，因為完全無法取得任何資料。

![image-20241005230731585](../images/2022/image-20241005230731585.png)

沒有取得任何錯誤的訊息，但是裡面完全**就是空的，是空的。**

# Debug 1: 試著把一樣的程式碼放在 Colab

可以參考一下以下的 [gist 程式碼](https://gist.github.com/kkdai/4d613dcdc86bad995477be4d22a7f907)。

![image-20241005230200026](../images/2022/image-20241005230200026.png)

原以為是程式碼的問題，看起來在 Colab 卻是可以正確地執行。看起來得使用 [GoogleApiYoutubeLoader](https://python.langchain.com/api_reference/community/document_loaders/langchain_community.document_loaders.youtube.GoogleApiYoutubeLoader.html) 。

# 在 Google CloudRun 上面使用 LangChain Youtube Loader

試著要在 CloudRun 使用  [GoogleApiYoutubeLoader](https://python.langchain.com/api_reference/community/document_loaders/langchain_community.document_loaders.youtube.GoogleApiYoutubeLoader.html)  需要有一些流程：

- 取得 System Account 的 json 檔案。 （參考[這篇文章](https://www.evanlin.com/til-heroku-gcp-key/))
  - 將 json 內容放在 [Secret Manager](https://cloud.google.com/security/products/secret-manager?hl=zh-TW) 來存放資料。
  - 程式碼中透過  [Secret Manager](https://cloud.google.com/security/products/secret-manager?hl=zh-TW) 來取得資料。
  - 呼叫 YouTube 程式碼。
-  透過以下的方式可以在 [Google Cloud 啟動 YouTube API](https://console.developers.google.com/apis/api/youtube.googleapis.com/overview?project=660825558664)

![image-20241005231500039](../images/2022/image-20241005231500039.png)

- [啟動 Secret Manager API](https://console.cloud.google.com/apis/library/secretmanager.googleapis.com) 來管理 System Account Key Json 檔案資料。

![image-20241006001807126](../images/2022/image-20241006001807126.png)

## 透過 Secret Manager 取得資料

- 首先需要設定 Project ID 在環境變數 `PROJECT_ID`。
- 然後事先將資料放在 `youtube_api_credentials` 之中。

```
def get_secret(secret_id):
    logging.debug(f"Fetching secret for: {secret_id}")
    client = secretmanager.SecretManagerServiceClient()
    name = f"projects/{os.environ['PROJECT_ID']}/secrets/{secret_id}/versions/latest"
    response = client.access_secret_version(request={"name": name})
    secret_data = response.payload.data.decode("UTF-8")
    logging.debug(f"Secret fetched successfully for: {secret_id}, {secret_data[:50]}")
    return secret_data
```

這樣就可以透過  [Secret Manager](https://cloud.google.com/security/products/secret-manager?hl=zh-TW)  來安全的取得資料。



## 試著先寫出一個範例程式碼部署到 GCP CloudRun

程式碼： [https://github.com/kkdai/gcp-test-youtuber]( https://github.com/kkdai/gcp-test-youtuber)

