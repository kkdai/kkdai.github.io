---
layout: post
title: "[學習心得][Python] 透過 LangChain 來處理特殊的中央氣象局資料"
description: ""
category: 
- 學習文件
- TodayILearn
tags: ["Langchain", "TIL", "Python"]
---

<img src="../images/2022/image-20230622194316256.png" alt="image-20230622194316256" style="zoom:50%;" />

# 前提

接下來，我們將介紹如何透過 LangChain 來打造一些實用的 LINE Bot 。第一個我們先透過旅遊小幫手的概念，來幫大家串接關於氣象的 LangChain Function Agent吧。

本篇文章將要帶給各位一些概念：

- 如何用 LangChain 來串接 中央氣象局的 Open API Data
- 如何在 LLM 的對話中來處理一些特別數值的輸入。
- 如何活用 LLM 對話的聰明性，來活用 LangChain Function Agent 。

這裡也列出一系列，我有撰寫關於 LangChain 的學習文章：

- [[學習心得][Python] 透過 LangChain 來處理特殊的中央氣象局資料]()

- [[學習心得\][Python] 透過 LangChain 的 Functions Agent 達成用中文來操控資料夾](https://www.evanlin.com/langchain-function-agent/)
- [[學習心得\][Python] 透過 LangChain 打造一個股價查詢 LINEBot - 股價小幫手](https://www.evanlin.com/linebot-langchain/)[](https://www.evanlin.com/langchain-function-agent/)

## 直接看如何使用程式碼 [https://github.com/kkdai/langchain_tools/blob/master/travel_tool/](https://github.com/kkdai/langchain_tools/blob/master/travel_tool/)

# 如何用 LangChain 來串接 中央氣象局的 Open API Data

要申請中央氣象局 OpenAPI Data ，依照以下流程:

- 到[官方網站](https://opendata.cwb.gov.tw/userLogin)註冊
- 註冊之後，透過[取得授權碼](https://opendata.cwb.gov.tw/user/authkey)。
- 透過官方 [OpenAPI 頁面](https://opendata.cwb.gov.tw/dist/opendata-swagger.html#/%E9%A0%90%E5%A0%B1/get_v1_rest_datastore_F_C0032_001)，來使用 （其實只要用一般天氣預報（全地區）就足夠了）

![image-20230622195316423](../images/2022/image-20230622195316423.png)



接下來來透過 ChatGPT 來協助撰寫相關資料：

- 透過 Execute 結果中的 CURL `

```bash
  curl -X 'GET' \
    'https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001?Authorization=你的授權碼' \
    -H 'accept: application/json'
```

- 然後去ChatGPT 問說，就可以取得 get_weather_data ，這邊記住我們只有使用地點。原因如下：

  - 雖然會傳回未來三天的氣象資料，但是 LLM 可以幫我們整理，並且篩選。
  - 三天的資料也可以幫助我們來處理更多詢問，不需要再查詢的時候就修改。

  ```python
  def get_weather_data(location_name=None):
  
      url = 'https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001'
  
      headers = {'accept': 'application/json'}
  
      query = {
          'Authorization': cwb_token}
  
      if location_name is not None:
          query['locationName'] = location_name
  
      response = requests.get(url, headers=headers, params=query)
  
      if response.status_code == 200:
          return response.json()
      else:
          return response.status_code
  ```

## 轉換成 LangChain Function Agent

參考之前其他的 Tool Agent 的寫法，可以很快速改成以下方式。

```python
cwb_token = os.getenv('CWB_TOKEN', None)

# From CWB API
# https://opendata.cwb.gov.tw/dist/opendata-swagger.html#/%E9%A0%90%E5%A0%B1/get_v1_rest_datastore_F_C0032_001
class WeatherDataInput(BaseModel):
    """Get weather data input parameters."""
    location_name: str = Field(...,
                               description="The cities in Taiwan.")


class WeatherDataTool(BaseTool):
    name = "get_weather_data"
    description = "Get the weather data for Taiwan"

    def _run(self,  location_name: str):
        weather_data_results = get_weather_data(
            location_name)

        return weather_data_results

    def _arun(self, location_name: str):
        raise NotImplementedError("This tool does not support async")

    args_schema: Optional[Type[BaseModel]] = WeatherDataInput
```

# 如何在 LLM 的對話中來處理一些特別數值的輸入。

如果你依照以上的方式來寫，你會發現你執行天氣的結果都會失敗。尤其是查詢台北市的天氣。如果回去看原來的 Open API Spec 

![image-20230622200638227](../images/2022/image-20230622200638227.png)

誒？！ 原來是固定輸入的資料，而且資料名稱用「臺北市」？！那該怎麼辦呢？

你不能要求每次叫使用者輸入「臺北市」，更不要自己去改那些輸入。（那就太不 LLM 了）。那該如何處理呢？

# 結語

