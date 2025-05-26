---
layout: post
title: "[Golang] 將 PTT 資料爬蟲改用 Firecrawl API 的研究與實作"
description: ""
category: 
- TIL
tags: ["Golang"]
---

# 將 PTT 資料爬蟲改用 Firecrawl API 的研究與實作

## 前提
`photomgr` 專案（https://github.com/kkdai/photomgr）一直以來都是爬取 PTT Beauty 板資料的得力工具，幫不少人抓取正妹板的文章和圖片。原本我們是用 Google Cloud Platform（GCP）直接連到 PTT 網站抓資料，簡單又順手。不過最近 PTT 把資料架到 Cloudflare 保護後，GCP 的連線常常被擋，可能是因為 Cloudflare 的防爬蟲機制（像是驗證碼或 IP 限制）太強大，導致原本的爬蟲程式完全 GG。為了讓 `photomgr` 繼續運作，我們研究了幾個替代方案，最後決定改用 Firecrawl API 來爬 PTT 的資料。這篇部落格會講解我們為什麼要做這個改變、怎麼改，以及改完後的程式碼和成果。

## 相關變動
PTT 最近開始用 Cloudflare 保護網站，加入了像是驗證碼、IP 限制等防爬蟲機制。這些措施讓我們原本用 GCP 直接送 HTTP 請求去抓資料的方式行不通，因為 Cloudflare 會把 GCP 的請求擋掉或限速，導致爬蟲常常失敗。經過一番研究，我們發現 Firecrawl 這個服務很適合解決這個問題。它不僅能繞過 Cloudflare 的防爬，還能把網頁內容轉成乾淨的 markdown 格式，方便我們解析。於是，我們決定把 `photomgr` 的爬蟲邏輯從原本的直接連線改成用 Firecrawl API 來抓 PTT Beauty 板的文章列表和內文。

## 什麼是 Firecrawl？
Firecrawl 是一個專為網頁爬蟲設計的 API 服務（https://www.firecrawl.dev/），可以幫你抓取網頁內容並轉成結構化的格式，比如 markdown 或 JSON。它最大的優勢是能處理像 Cloudflare 這種防爬蟲保護，還能模擬瀏覽器行為，抓到動態載入的內容。Firecrawl 的 `/v1/scrape` 端點讓我們可以送一個網址過去，它就會回傳網頁的主要內容，省去自己處理複雜 HTML 的麻煩。對我們來說，這是個超佛心的工具，因為它讓爬蟲變得更穩定，還能省下不少寫解析邏輯的時間。

## 要如何修改？
為了讓 `photomgr` 用上 Firecrawl API，我們需要把原本 `ptt.go` 的爬蟲邏輯改成呼叫 Firecrawl 的 API。以下是改動的重點：

1. **保留公開 API 不變**：`ptt.go` 裡的公開函數（像是 `GetPosts` 和 `GetPostDetails`）已經被其他使用者依賴，所以我們不能改動這些函數的輸入輸出格式，只能改內部的實作邏輯。
2. **使用 Firecrawl API**：改用 Firecrawl 的 `/v1/scrape` 端點來抓 PTT Beauty 板的列表頁（https://www.ptt.cc/bbs/Beauty/index.html）和單篇文章頁（像是 https://www.ptt.cc/bbs/Beauty/M.1748080032.A.015.html）。
3. **解析 markdown 資料**：Firecrawl 回傳的資料是 markdown 格式，我們需要把它解析成結構化的 JSON，像是文章標題、網址、作者、日期、推文數（或「爆」標記）等。
4. **環境變數管理 API Key**：Firecrawl 的 API 需要一個 key，我們會從環境變數 `FIRECRAWL_KEY` 讀取，確保安全不硬寫在程式碼裡。
5. **單元測試**：新增單元測試來驗證爬蟲和解析邏輯，目標是至少 80% 的程式碼覆蓋率。

### 關於 over18=1 的 Cookie 寫法
PTT Beauty 板有年齡限制，瀏覽時需要設定 `over18=1` 的 Cookie 來通過檢查。在 Firecrawl API 的請求中，我們需要在 `headers` 裡加入這個 Cookie，這樣才能順利抓到頁面內容。具體寫法如下：

```json
{
  "url": "https://www.ptt.cc/bbs/Beauty/index.html",
  "headers": {
    "Cookie": "over18=1",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
  },
  "formats": ["markdown"],
  "onlyMainContent": true,
  "waitFor": 1000
}
```

這段 JSON 告訴 Firecrawl 在送請求時要帶上 over18=1 的 Cookie，模擬一個通過年齡驗證的使用者。User-Agent 則是模擬瀏覽器，確保 PTT 伺服器不會因為奇怪的請求頭而擋掉我們。

相關程式碼

以下是改用 Firecrawl API 的一些核心程式碼範例，展示如何抓取和解析 PTT Beauty 板的資料：

抓取列表頁

go

```go
package ptt

import (
    "encoding/json"
    "net/http"
    "os"
)

type FirecrawlResponse struct {
    Success bool `json:"success"`
    Data    struct {
        Markdown string `json:"markdown"`
    } `json:"data"`
}

func GetPosts() ([]Post, error) {
    apiKey := os.Getenv("FIRECRAWL_KEY")
    if apiKey == "" {
        return nil, errors.New("FIRECRAWL_KEY is not set")
    }

    url := "https://www.ptt.cc/bbs/Beauty/index.html"
    reqBody := map[string]interface{}{
        "url": url,
        "headers": map[string]string{
            "Cookie":     "over18=1",
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
        },
        "formats":       []string{"markdown"},
        "onlyMainContent": true,
        "waitFor":       1000,
    }

    body, _ := json.Marshal(reqBody)
    req, _ := http.NewRequest("POST", "https://api.firecrawl.dev/v1/scrape", bytes.NewBuffer(body))
    req.Header.Set("Authorization", "Bearer "+apiKey)
    req.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    var fcResp FirecrawlResponse
    json.NewDecoder(resp.Body).Decode(&fcResp)
    if !fcResp.Success {
        return nil, errors.New("Firecrawl API request failed")
    }

    // 解析 markdown 成 Post 結構
    posts, err := parseIndexMarkdown(fcResp.Data.Markdown)
    if err != nil {
        return nil, err
    }
    return posts, nil
}

func parseIndexMarkdown(markdown string) ([]Post, error) {
    // 使用正則表達式或 markdown 解析庫解析
    // 範例：提取標題、網址、作者、日期、推文數
    var posts []Post
    // 假設 Post 結構如下
    type Post struct {
        Title     string
        URL       string
        Author    string
        Date      string
        PushCount int
    }
    // 實作解析邏輯（簡化範例）
    lines := strings.Split(markdown, "\n")
    for _, line := range lines {
        if strings.Contains(line, "[正妹]") || strings.Contains(line, "[公告]") {
            // 解析標題、網址等
            post := Post{ /* 填入解析結果 */ }
            posts = append(posts, post)
        }
    }
    return posts, nil
}
```

抓取單篇文章

go

```go
func GetPostDetails(url string) (PostDetail, error) {
    apiKey := os.Getenv("FIRECRAWL_KEY")
    if apiKey == "" {
        return PostDetail{}, errors.New("FIRECRAWL_KEY is not set")
    }

    reqBody := map[string]interface{}{
        "url": url,
        "headers": map[string]string{
            "Cookie":     "over18=1",
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
        },
        "formats":       []string{"markdown"},
        "onlyMainContent": true,
        "waitFor":       1000,
    }

    body, _ := json.Marshal(reqBody)
    req, _ := http.NewRequest("POST", "https://api.firecrawl.dev/v1/scrape", bytes.NewBuffer(body))
    req.Header.Set("Authorization", "Bearer "+apiKey)
    req.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        return PostDetail{}, err
    }
    defer resp.Body.Close()

    var fcResp FirecrawlResponse
    json.NewDecoder(resp.Body).Decode(&fcResp)
    if !fcResp.Success {
        return PostDetail{}, errors.New("Firecrawl API request failed")
    }

    // 解析 markdown 成 PostDetail 結構
    detail, err := parsePostMarkdown(fcResp.Data.Markdown)
    if err != nil {
        return PostDetail{}, err
    }
    return detail, nil
}

func parsePostMarkdown(markdown string) (PostDetail, error) {
    // 假設 PostDetail 結構如下
    type PostDetail struct {
        Author    string
        Board     string
        Title     string
        Date      string
        ImageURLs []string
        Content   string
    }
    // 實作解析邏輯（簡化範例）
    var detail PostDetail
    lines := strings.Split(markdown, "\n")
    for _, line := range lines {
        if strings.HasPrefix(line, "作者") {
            detail.Author = strings.TrimPrefix(line, "作者")
        } else if strings.Contains(line, "https://i.imgur.com") {
            detail.ImageURLs = append(detail.ImageURLs, line)
        } // 其他欄位解析
    }
    return detail, nil
}
```

單元測試範例

go

```go
package ptt

import (
    "testing"
    "github.com/stretchr/testify/assert"
)

func TestGetPosts(t *testing.T) {
    // 模擬 Firecrawl API 回應
    mockMarkdown := `
[正妹] OOO
jerryyuan
5/24
[搜尋同標題文章](https://www.ptt.cc/bbs/Beauty/search?q=thread%3A%5B%E6%AD%A3%E5%A6%B9%5D)
    `
    posts, err := parseIndexMarkdown(mockMarkdown)
    assert.NoError(t, err)
    assert.Len(t, posts, 1)
    assert.Equal(t, "[正妹] OOO", posts[0].Title)
}
```

## 總結

PTT 改用 Cloudflare 保護後，讓我們原本的 GCP 爬蟲方案直接陣亡，逼得我們得找新方法來抓資料。Firecrawl API 成了我們的救星，不僅能繞過 Cloudflare 的防爬，還能把 PTT 的頁面轉成乾淨的 markdown，省下不少解析的麻煩。我們改進了 photomgr 的 ptt.go，用 Firecrawl API 抓取 Beauty 板的列表和文章，保留了原本的公開 API 介面，確保現有使用者不受影響。透過環境變數管理 API Key、加上單元測試，程式碼安全又穩定。這次遷移讓我們學到如何應對網站保護機制的挑戰，也證明 Firecrawl 是爬蟲任務的超強幫手。未來我們會持續監控 PTT 的變化，確保爬蟲能一直順利跑下去！

