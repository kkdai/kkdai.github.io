---
layout: post
title: "[Go] 用 Go + Gemini + GCP 打造智慧垃圾車 LINE Bot：從查詢到提醒的完整解決方案"
description: "使用 Go 語言結合 LINE Bot SDK、Gemini AI、Google Maps API 和 Firestore，打造一個能理解自然語言、提供即時查詢和智慧提醒的垃圾車資訊 Bot"
category: 
- TIL
tags: ["Go", "LINE Bot", "GCP", "Gemini", "Firestore", "Cloud Run"]

---

<img src="../images/image-20251102143236468.png" alt="image-20251102143236468" style="zoom:50%;" />



# 前情提要

在台灣，垃圾車的時間總是讓人捉摸不定。明明記得昨天是晚上七點來，今天卻遲遲等不到；或是剛好外出倒垃圾，垃圾車就這樣錯過了。相信這是許多人共同的困擾。

隨著智慧城市的發展，越來越多城市開始提供垃圾車即時資訊 API，但這些資料對一般民眾來說並不容易使用。

這時候我看到臉書上一個[朋友貼文](https://www.facebook.com/yukaihuangtw/posts/pfbid02Hf5K28V7BmBcy9FzHBdu9r8zD5TjtK3MTKL4BpwMdX34Wc9SP1ktoZfvTGQTRix5l) ，他敘述他做出了一個垃圾車追蹤的網站。 ([網站](https://garbage.yukai.dev/)， [github](https://github.com/Yukaii/garbage/))

<img src="../images/image-20251102143208176.png" alt="image-20251102143208176" style="zoom:25%;" />

這時候我在想，難道不能結合 LINE Bot 做出一個可以很快速幫助到其他的工具嗎?因此，我決定打造一個垃圾車 LINE Bot，讓大家可以透過最熟悉的通訊軟體，輕鬆查詢垃圾車資訊，甚至設定提醒通知。更重要的是，這個 Bot 不只是簡單的指令查詢，而是整合了 Google Gemini AI，能夠理解「我晚上七點前在哪裡倒垃圾？」這樣的自然語言，提供真正智慧化的服務體驗。



### 專案程式碼：

[https://github.com/kkdai/linebot-garbage-helper](https://github.com/kkdai/linebot-garbage-helper)

（透過這個程式碼，可以快速部署到 GCP Cloud Run，並使用 Cloud Build 實現自動化 CI/CD）

## 🗑️ 專案功能介紹

### 核心功能

1. **🗑️ 即時查詢垃圾車**
   - 輸入地址或分享位置即可查詢附近垃圾車站點
   - 顯示預計抵達時間、路線資訊和 Google Maps 導航連結

2. **⏰ 智慧提醒系統**
   - 可設定垃圾車抵達前 N 分鐘提醒
   - 自動推播通知，再也不會錯過垃圾車
   - 支援多種提醒狀態管理（活躍、已發送、已過期、已取消）

3. **🤖 自然語言查詢**
   - 整合 Google Gemini AI，支援自然語言理解
   - 例如：「我晚上七點前在台北市大安區哪裡倒垃圾？」
   - 自動提取地點、時間範圍等查詢條件

4. **❤️ 收藏地點功能**
   - 儲存常用地點（家、公司等）
   - 快速查詢收藏地點的垃圾車資訊

## 🏗️ 技術架構說明

### 為什麼選擇 Go 語言？

1. **卓越的併發處理**：Go 的 goroutine 非常適合處理大量的 webhook 請求
2. **快速的編譯和部署**：特別適合 Cloud Run 的容器化部署
3. **豐富的生態系**：LINE Bot SDK、Google Cloud SDK 都有官方支援
4. **優秀的效能**：記憶體使用量低，啟動速度快

### 系統架構圖

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   LINE Client   │───▶│    Cloud Run     │───▶│   Firestore     │
└─────────────────┘    │  (Go App)        │    │   (Database)    │
                       └──────────────────┘    └─────────────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │  External APIs   │
                    │  • Google Maps   │
                    │  • Gemini AI     │
                    │  • 垃圾車資料源   │
                    └──────────────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │ Cloud Scheduler  │
                    │ (提醒排程觸發)    │
                    └──────────────────┘
```

### 主要技術棧

- **語言**: Go 1.24
- **雲端平台**: Google Cloud Platform
- **資料庫**: Firestore（NoSQL 文件資料庫）
- **外部 API**: LINE Bot SDK, Google Maps API, Gemini API
- **資料來源**: [Yukaii/garbage](https://github.com/Yukaii/garbage)
- **部署**: Cloud Run + Cloud Build

## 💻 核心功能實作

### 1. LINE Webhook 處理

首先來看看如何處理 LINE 的 webhook 事件：

```go
func (h *Handler) HandleWebhook(w http.ResponseWriter, r *http.Request) {
    log.Printf("Webhook received from %s", r.RemoteAddr)
    
    events, err := webhook.ParseRequest(h.channelSecret, r)
    if err != nil {
        log.Printf("Error parsing webhook request: %v", err)
        http.Error(w, "Bad Request", http.StatusBadRequest)
        return
    }
    
    ctx := r.Context()
    for _, event := range events {
        go h.handleEvent(ctx, event)  // 使用 goroutine 處理事件
    }
    
    w.WriteHeader(http.StatusOK)
}
```

### 2. Gemini AI 自然語言理解

這是整個系統最有趣的部分，透過 Gemini 來理解使用者的自然語言查詢：

```go
func (gc *GeminiClient) AnalyzeIntent(ctx context.Context, userMessage string) (*IntentResult, error) {
    model := gc.client.GenerativeModel(gc.model)
    
    prompt := fmt.Sprintf(`你是一個查詢意圖分析器，專門分析使用者關於垃圾車的查詢。

使用者輸入可能包含地名與時間。請分析輸入並輸出 JSON 格式的結果。

輸出格式：
{
  "district": "地區名稱（如果有的話）",
  "time_window": {
    "from": "開始時間（HH:MM格式，如果有的話）",
    "to": "結束時間（HH:MM格式，如果有的話）"
  },
  "keywords": ["關鍵字陣列"],
  "query_type": "garbage_truck_eta"
}

範例：
輸入：「我晚上七點前在台北市大安區哪裡倒垃圾？」
輸出：
{
  "district": "台北市大安區",
  "time_window": {
    "from": "",
    "to": "19:00"
  },
  "keywords": ["台北市", "大安區", "倒垃圾", "晚上", "七點"],
  "query_type": "garbage_truck_eta"
}

請分析以下使用者輸入：
「%s」

請只回傳 JSON，不要包含其他說明文字。`, userMessage)

    resp, err := model.GenerateContent(ctx, genai.Text(prompt))
    if err != nil {
        return nil, err
    }
    
    // 解析 JSON 回應
    var result IntentResult
    if err := json.Unmarshal([]byte(responseText), &result); err != nil {
        // 如果 Gemini 無法解析，回退到簡單的關鍵字比對
        return &IntentResult{
            District:  extractDistrict(userMessage),
            Keywords:  []string{userMessage},
            QueryType: "garbage_truck_eta",
        }, nil
    }
    
    return &result, nil
}
```

### 3. 智慧提醒排程系統

提醒系統是這個專案的核心功能之一，設計上考慮了可靠性和效能：

```go
func (s *Scheduler) ProcessReminders(ctx context.Context) error {
    now := time.Now()

    // 效能優化：先檢查是否有活躍提醒
    count, err := s.store.CountActiveReminders(ctx)
    if err != nil {
        log.Printf("Warning: failed to count active reminders: %v", err)
    } else if count == 0 {
        log.Printf("No active reminders, skipping processing")
        return nil
    }

    reminders, err := s.store.GetActiveReminders(ctx, now)
    if err != nil {
        return fmt.Errorf("failed to get active reminders: %w", err)
    }

    log.Printf("Found %d active reminders to process", len(reminders))

    for _, reminder := range reminders {
        notificationTime := reminder.ETA.Add(-time.Duration(reminder.AdvanceMinutes) * time.Minute)
        
        // 檢查是否到了發送提醒的時間
        if now.Before(notificationTime) {
            continue  // 還不到發送時間
        }

        if now.After(reminder.ETA) {
            // ETA 已過期，標記為過期
            s.store.UpdateReminderStatus(ctx, reminder.ID, "expired")
            continue
        }

        // 發送提醒通知
        if err := s.sendReminderNotification(ctx, reminder); err != nil {
            log.Printf("Error sending reminder %s: %v", reminder.ID, err)
            continue
        }

        // 更新狀態為已發送
        s.store.UpdateReminderStatus(ctx, reminder.ID, "sent")
    }

    return nil
}
```

### 4. Firestore 資料結構設計

我們使用 Firestore 來儲存使用者資料和提醒資訊：

```go
type Reminder struct {
    ID             string    `firestore:"id"`
    UserID         string    `firestore:"userId"`
    StopName       string    `firestore:"stopName"`
    RouteID        string    `firestore:"routeId"`
    ETA            time.Time `firestore:"eta"`
    AdvanceMinutes int       `firestore:"advanceMinutes"`
    Status         string    `firestore:"status"`  // active, sent, expired, cancelled
    CreatedAt      time.Time `firestore:"createdAt"`
    UpdatedAt      time.Time `firestore:"updatedAt"`
}

type User struct {
    ID        string     `firestore:"id"`
    Favorites []Favorite `firestore:"favorites"`
    CreatedAt time.Time  `firestore:"createdAt"`
    UpdatedAt time.Time  `firestore:"updatedAt"`
}
```

### 5. 雙重保障的提醒機制

為了確保提醒不會遺漏，系統設計了雙重保障機制：

1. **本地排程器**：應用啟動時自動開始背景排程服務
2. **外部觸發**：透過 Cloud Scheduler 定期調用 `/tasks/dispatch-reminders`

```go
// 本地排程器
func (s *Scheduler) StartScheduler(ctx context.Context) {
    ticker := time.NewTicker(1 * time.Minute)
    defer ticker.Stop()

    cleanupTicker := time.NewTicker(1 * time.Hour)
    defer cleanupTicker.Stop()

    for {
        select {
        case <-ctx.Done():
            return
        case <-ticker.C:
            s.ProcessReminders(ctx)
        case <-cleanupTicker.C:
            s.CleanupExpiredReminders(ctx)
        }
    }
}

// 外部觸發端點
r.HandleFunc("/tasks/dispatch-reminders", func(w http.ResponseWriter, r *http.Request) {
    token := r.Header.Get("Authorization")
    if token != "Bearer "+cfg.InternalTaskToken {
        http.Error(w, "Unauthorized", http.StatusUnauthorized)
        return
    }

    if err := reminderService.ProcessReminders(r.Context()); err != nil {
        log.Printf("Error processing reminders: %v", err)
        http.Error(w, "Internal Server Error", http.StatusInternalServerError)
        return
    }

    w.WriteHeader(http.StatusOK)
})
```

## 🚀 Cloud Build 自動化部署

### 設定 Cloud Build 觸發器

部署流程完全自動化，只要推送程式碼到 main 分支，就會自動觸發部署：

```yaml
# cloudbuild.yaml
steps:
  # Build the container image
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build', '-t', 'gcr.io/$PROJECT_ID/garbage-linebot:$COMMIT_SHA', '.']

  # Push the container image to Container Registry
  - name: 'gcr.io/cloud-builders/docker'
    args: ['push', 'gcr.io/$PROJECT_ID/garbage-linebot:$COMMIT_SHA']

  # Deploy container image to Cloud Run
  - name: 'gcr.io/cloud-builders/gcloud'
    args:
    - 'run'
    - 'deploy'
    - 'garbage-linebot'
    - '--image'
    - 'gcr.io/$PROJECT_ID/garbage-linebot:$COMMIT_SHA'
    - '--region'
    - 'asia-east1'
    - '--platform'
    - 'managed'
    - '--allow-unauthenticated'
    - '--set-env-vars'
    - 'LINE_CHANNEL_SECRET=${_LINE_CHANNEL_SECRET},LINE_CHANNEL_ACCESS_TOKEN=${_LINE_CHANNEL_ACCESS_TOKEN},GOOGLE_MAPS_API_KEY=${_GOOGLE_MAPS_API_KEY},GEMINI_API_KEY=${_GEMINI_API_KEY},GCP_PROJECT_ID=$PROJECT_ID'
```

### 環境變數設定

在 Cloud Build 觸發器中設定替代變數：

```
_LINE_CHANNEL_SECRET: your_line_channel_secret
_LINE_CHANNEL_ACCESS_TOKEN: your_line_channel_access_token
_GOOGLE_MAPS_API_KEY: your_google_maps_api_key
_GEMINI_API_KEY: your_gemini_api_key
```

特別的是，`INTERNAL_TASK_TOKEN` 會在應用啟動時自動生成，無需手動設定！

## 🔧 遇到的挑戰與解決方案

### 1. Gemini API 的穩定性處理

**問題**：Gemini API 偶爾會回傳非 JSON 格式的回應，導致解析失敗。

**解決方案**：實作錯誤處理和回退機制：

```go
var result IntentResult
if err := json.Unmarshal([]byte(responseText), &result); err != nil {
    // 如果 Gemini 無法解析，回退到簡單的關鍵字比對
    return &IntentResult{
        District:  extractDistrict(userMessage),
        Keywords:  []string{userMessage},
        QueryType: "garbage_truck_eta",
        TimeWindow: TimeWindow{From: "", To: ""},
    }, nil
}
```

### 2. Firestore 查詢效能優化

**問題**：每次都查詢所有活躍提醒會造成不必要的資料讀取。

**解決方案**：加入 count 查詢作為早期回傳優化：

```go
// 先檢查是否有活躍提醒
count, err := s.store.CountActiveReminders(ctx)
if count == 0 {
    log.Printf("No active reminders, skipping processing")
    return nil
}
```

### 3. Cloud Scheduler 區域設定問題

**問題**：Cloud Scheduler 在某些區域可能不支援，導致自動提醒失效。

**解決方案**：設計雙重保障機制，即使外部排程器失效，本地排程器仍能正常運作。

### 4. LINE Bot 訊息推播限制

**問題**：LINE Bot 有推播訊息的頻率限制。

**解決方案**：
- 實作提醒狀態管理，避免重複發送
- 加入適當的錯誤處理和重試機制
- 使用 goroutine 非同步處理，避免阻塞主要流程

## 📊 效能監控與可靠性

### 健康檢查端點

```go
r.HandleFunc("/healthz", func(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(http.StatusOK)
    w.Write([]byte("OK"))
}).Methods("GET")
```

### 詳細的日誌記錄

系統在關鍵節點都有詳細的日誌記錄，方便除錯和監控：

```go
log.Printf("Processing reminder %s: ETA=%s, NotificationTime=%s, AdvanceMinutes=%d",
    reminder.ID, reminder.ETA.Format("2006-01-02 15:04:05"),
    notificationTime.Format("2006-01-02 15:04:05"), reminder.AdvanceMinutes)
```

## 🎯 總結與未來改進

這個垃圾車 LINE Bot 專案展示了如何結合現代化的技術棧來解決日常生活中的實際問題。透過 Go 語言的高效能、Gemini AI 的自然語言理解、以及 GCP 的雲端服務，我們打造了一個既實用又智慧的解決方案。

### 專案亮點

1. **智慧化查詢**：透過 Gemini AI 理解自然語言，提供更友善的使用體驗
2. **可靠的提醒系統**：雙重保障機制確保重要通知不會遺漏
3. **現代化架構**：使用微服務架構，易於擴展和維護
4. **自動化部署**：完整的 CI/CD 流程，降低維運成本

### 未來改進方向

1. **效能優化**
   - 建立 Firestore 複合索引提升查詢效能
   - 實作批量推播減少 API 調用

2. **可靠性提升**
   - 加入分布式鎖避免重複執行
   - 實作指數退避重試機制

3. **功能擴展**
   - 支援更多城市的垃圾車資料
   - 加入使用統計和分析功能
   - 整合更多 AI 能力，如圖片識別

4. **監控強化**
   - 整合 Prometheus/OpenTelemetry
   - 建立完整的效能監控儀表板

透過這個專案，我深刻體會到 Go 語言在雲端原生應用開發上的優勢，以及 AI 技術如何讓傳統應用變得更加智慧化。希望這個經驗分享能夠幫助到正在學習相關技術的開發者們！

### 相關資源

- [專案 GitHub Repository](https://github.com/kkdai/linebot-garbage-helper)
- [LINE Bot SDK for Go](https://github.com/line/line-bot-sdk-go)
- [Google Gemini AI API](https://ai.google.dev/)
- [GCP Cloud Run 文件](https://cloud.google.com/run/docs)
- [Firestore 文件](https://cloud.google.com/firestore/docs)