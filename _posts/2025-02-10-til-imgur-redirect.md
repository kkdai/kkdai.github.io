---
layout: post
title: "[Python/Golang] 解決 Imgur 圖片下載轉址問題"
description: ""
category: 
- TIL
tags: ["imgur", "python"]

---

![Google Chrome 2025-02-12 08.33.04](../images/2022/Google Chrome 2025-02-12 08.33.04.png)



## **最近的變動造成的問題**

近期，Imgur 對其圖片連結進行了技術更新，導致直接訪問 `i.imgur.com/{image_id}.jpeg` 類型的 URL 時會被重定向到 `imgur.com/{image_id}` 頁面。這種行為使得在應用程式或網頁中直接顯示或下載圖片變得困難。此外，這種轉址機制也會影響到使用者腳本和瀏覽器擴充套件的正常運作，使得原本能夠阻止轉址的工具可能失效。因此，開發者需要找到新的方法來繞過這個限制。

## **解決思路**

要解決這個問題，我們可以採用以下幾種策略：

1. **設置正確的 HTTP 標頭**特別是設定 `Referer` 標頭，以模擬從 Imgur 頁面訪問圖片。使用合理的 `User-Agent` 標頭，以避免被視為爬蟲請求。
2. **使用 Imgur 官方 API**透過官方 API 可以穩定地取得圖片直連 URL，但需要註冊並取得 Client ID 和 Client Secret。這種方法通常更可靠，但需要額外的手續和速率限制考量。
3. **瀏覽器擴充套件或腳本更新**更新 NoImgurRedirect 等擴充套件以適應最新變動，或撰寫自訂腳本來處理轉址問題。

## **範例程式碼**

以下是一段 Python 程式碼示範如何設定正確標頭來下載 Imgur 圖片：

```
python
import requests

def download_img(image_id):
    # 構建 URL
    direct_url = f"https://i.imgur.com/{image_id}.jpeg"
    referer_url = f"https://imgur.com/{image_id}"
    
    # 設置請求頭
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
        "Referer": referer_url
    }
    
    try:
        response = requests.get(direct_url, headers=headers)
        response.raise_for_status()
        
        if 'image' in response.headers.get('Content-Type', ''):
            filename = f"{image_id}.jpeg"
            with open(filename, 'wb') as f:
                f.write(response.content)
            print(f"圖片已成功下載：{filename}")
        else:
            print("下載失敗：返回內容不是圖片")
    
    except requests.RequestException as e:
        print(f"下載失敗：{e}")

# 測試下載（假設 image_id 為「example123」）
download_img("example123")
```

## **未來該如何小心類似的問題**

1. **關注平台更新通知**
   定期查看 Imgur 的官方公告和社群反饋，以便及時了解任何技術變動。
2. **維護現有工具與庫版本**
   保持瀏覽器擴充套件、腳本以及相關庫版本是最新的，以確保它們能夠適應新出現的情況。
3. **評估替代方案與備選策略**
   考慮使用其他第三方服務作為備選方案，並在必要時切換至不同的存儲平台以減少風險依賴性。
4. **遵守服務條款與政策**
   在實施任何工作流程時，務必遵守相關平台提供者的服務條款與政策，以避免帳戶封禁等不利後果。
