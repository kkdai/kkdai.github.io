---
layout: post
title: "[I/O Extended Taipei] 在 Gemini API 家族中建構應用程式：從呼叫 API，到架構一個會自己完成工作的系統"
description: "2026 年的 Gemini API 已經不只是模型 API，而是一個包含 File Search、Agents API、Webhook、Batch、Live API 與 Context Caching 的應用平台。這篇整理 LINE 台灣開發者關係部技術總監 Evan Lin 在 I/O Extended Taipei 分享的核心觀察：開發者該從『怎麼 call model』，轉向『怎麼設計一個可路由、可非同步、可擴充的 AI 系統』。"
category:
- AI
- GCP
tags: ["Gemini API", "Gemini 3.5 Pro", "Gemini 3.5 Flash", "File Search", "Agents API", "Webhook", "Managed RAG", "AI Agent", "LINE Bot"]
---

![LINE 2026-06-12 16.06.10](../images/LINE 2026-06-12 16.06.10.tiff)

(活動：[Google I/O Extended 2026 Taipei](https://gdg.community.dev/events/details/google-gdg-taipei-presents-google-io-extended-2026-taipei/) / 簡報：[SpeakerDeck](https://speakerdeck.com/line_developers_tw/building-applications-in-the-gemini-api-family))

# 前情提要：Gemini API 已經不是「多打一個 prompt」而已

如果你對 Gemini API 的印象還停留在「選一個 model，送一段 prompt，拿回一段文字」，那你看到 2026 年這一輪更新時，很可能會突然意識到一件事：

**Gemini API 已經從單純的 API 介面，變成一個可以拿來搭應用、搭代理、搭非同步流程的完整平台。**

這篇內容整理自我在 [Google I/O Extended 2026 Taipei](https://gdg.community.dev/events/details/google-gdg-taipei-presents-google-io-extended-2026-taipei/) 的分享「在 Gemini API 家族中建構應用程式」。LINE 台灣開發者關係部技術總監 **Evan Lin** 在現場反覆強調的核心觀察是：開發者現在真正該思考的，不再只是 _「我要用 Pro 還是 Flash？」_，而是 _「我要怎麼把模型、檢索、代理、回呼與成本控制串成一套系統？」_。

換句話說，重點正在從 **call API**，轉向 **design system**。

---

## 先看全景圖：2026 Gemini API 家族到底多了什麼？

如果把 2026 年的 Gemini API 當成一張 capability map 來看，大致可以拆成三層。

### 第一層：核心模型

*   **Gemini 3.5 Pro**：最強推理能力，適合複雜規劃、進階分析與多步驟任務。
*   **Gemini 3.5 Flash**：主力模型，速度、成本與能力最平衡，適合多數產品流量。
*   **Flash-Lite**：高頻率、低成本場景的意圖判斷器與前置分類器。
*   **Gemini Embedding 2**：不只文本，也能支援多模態向量化需求。

### 第二層：關鍵能力模組

*   **Retrieval**：File Search、Google Search Grounding、URL Context。
*   **Agent / Async**：Agents API、Webhook、Deep Research agent。
*   **Infrastructure**：Context caching、Batch API、Live API。

### 第三層：系統設計方式

這一層反而最重要。因為當上面那幾個能力被做成平台服務之後，很多以前得自己補的「中間層」突然不見了：

*   不一定要自己搭一套 RAG pipeline。
*   不一定要自己養 agent loop。
*   不一定要用 polling 卡住主伺服器等結果。

**核心觀察**：Gemini API 的升級不只是「模型變強」，而是 **Google 把原本屬於應用層的麻煩事，往平台層往下吃掉了**。這會直接改變我們設計 AI 系統的方式。

---

# 架構轉折點：三個工具，三次思維切換

這場分享裡最值得反覆消化的，是這三個工具背後代表的架構變化。

## 1. File Search：從手刻 RAG，轉向 Managed RAG

以前講到企業知識問答，大家直覺就是：

1.  切 chunk。
2.  做 embedding。
3.  存進 vector DB。
4.  寫 retrieval code。
5.  再自己補 citation 與權限控管。

現在 File Search 出現後，開發者可以把更多力氣放在「文件怎麼治理、權限怎麼分、回答怎麼呈現」，而不是一直重複寫那套基礎設施。

更重要的是，它不是只會查文字。

### 為什麼這次的 File Search 特別值得注意？

*   **圖文同空間**：PDF 裡的截圖、圖表、圖文混排，不再只是附件，而是模型可理解的內容。
*   **Metadata 過濾**：可以依部門、系統、文件類型做過濾，這對企業內部知識檢索非常重要。
*   **精確引用**：能回到具體頁數與 grounding metadata，讓回答更能被信任。

這代表一件很實際的事：很多企業過去花在 LangChain、向量庫與 chunking 策略上的時間，現在可以大幅往 **權限設計、UX、內容治理** 轉移。

## 2. Agents API：從 client-side loop，轉向 server-side managed agent

過去要做 agent，常見寫法是自己維護一個 ReAct 或 tool loop：

*   模型想下一步。
*   呼叫工具。
*   收結果。
*   再餵回模型。
*   重複直到完成。

問題是這裡面充滿了工程細節：狀態保存、timeout、重試、背景執行、長任務監控，最後你會發現自己大半時間都在養一個「agent runtime」。

Agents API 改變的地方在於：你可以把任務 POST 給 Gemini，讓它在 server side 把長流程跑完，甚至處理到 20 分鐘等級的複雜任務。

這背後的意義不是「比較方便」而已，而是開發者終於可以把焦點放回：

*   任務如何定義？
*   哪些工具可以用？
*   成功條件是什麼？
*   結果回來後產品要怎麼接？

## 3. Webhook：從 Polling，轉向 Event-Driven

一旦任務可能跑到數分鐘，甚至十幾分鐘，傳統同步請求就不合理了。

所以 Webhook 的角色其實很關鍵：不是小功能，而是讓整套 agent workflow 能真正進入 production 的必要條件。當 Gemini 完工後主動把結果 POST 回你的 server，你的系統就能改成事件驅動：

*   前台先回使用者「任務已接收」。
*   背景交給 Agents API 執行。
*   完成後用 webhook 回推結果。
*   再由你的服務通知使用者、更新資料庫或觸發下一步流程。

這對高併發產品特別重要，因為你終於不需要拿一堆 server connection 在那裡傻等。

---

# 從 LINE Bot 的角度看，該怎麼設計一套 Gemini 應用？

Evan 在分享裡給的一個很實用的建議，是 **在 LLM 前面先放一層 router**。

這個設計聽起來簡單，但幾乎決定了你的成本、延遲與可預測性。

## 一個很務實的路由方式

先用便宜的 **Flash-Lite** 做意圖分流：

*   **快問快答**：直接交給 Flash 或 Flash-Lite 生成。
*   **查公司文件**：進 File Search。
*   **複雜長任務**：進 Agents API。

這麼做有三個好處：

1.  **先控成本**：不是每題都直接打最貴、最重的模型。
2.  **先控延遲**：簡單需求不要誤進長流程。
3.  **先控系統行為**：讓整體流程比「所有事都丟給一個大模型即興發揮」更穩定。

如果你是做 LINE Bot、客服助手、內部知識助理或工作流代理，這個 router 幾乎應該是預設配置，而不是之後再補。

---

## 基礎建設不是不重要，而是不用每次都自己重做

這場分享另一個很強的訊息是：開發者的時間應該重新分配。

以前很多 AI 專案的工時，其實被這些事吃掉：

*   向量資料庫維運
*   chunking 與 retrieval 調參
*   長任務排程
*   websocket / polling / callback 流程
*   token 成本優化

現在有了 File Search、Agents API、Webhook、Context caching、Batch API 之後，我們比較應該花時間的地方，變成：

*   業務規則與工具邊界
*   文件權限與資料治理
*   使用者互動體驗
*   任務拆解與路由策略
*   失敗回復與結果可解釋性

這也是為什麼我很認同 Evan 那句潛台詞：**真正有價值的，不是你會不會自己造一套向量庫，而是你能不能把 80% 的精力放回產品核心。**

---

## 三個最值得直接帶走的實戰結論

### 1. 在 LLM 之前，先放路由層

不要把所有問題都直接送進同一個模型。先分類，再決定是要生成、檢索，還是進 agent 任務。

### 2. 擁抱非同步，不要硬把長任務塞進同步 API

只要任務可能超過幾秒，就該認真考慮 Agents API + Webhook。這不是優化，而是架構正確性問題。

### 3. 把 RAG 的工程時間，換去做權限與體驗

當 File Search 已經能處理大量基礎工作時，開發者更該關心的是：資料能不能安全查、答案能不能被驗證、引用能不能讓使用者信任。

---

## 為什麼這場分享值得反覆看？

因為它點破了一個很多團隊現在正處在的轉折點：

**我們已經不再只是替 LLM 寫 prompt，而是在替 AI 應用設計作業系統。**

模型當然還是核心，但真正拉開產品差距的地方，越來越不是「你選哪個模型」，而是：

*   你怎麼決定何時用哪個能力。
*   你怎麼讓系統能長時間可靠地跑。
*   你怎麼讓答案可追溯、可驗證、可維運。

如果你還用 2024 年那套「一個 chat endpoint 包天下」的方式理解生成式 AI，那看到 2026 年的 Gemini API 家族，會很容易低估它。

---

## 後記：從 API 使用者，變成 AI 系統設計者

這場「在 Gemini API 家族中建構應用程式」最有價值的地方，不是再多教你一個新參數或新 SDK，而是提醒大家一個更根本的轉向：

**下一階段的競爭力，不在於誰比較會 call model，而在於誰比較會把 model、retrieval、agent 與 event flow 組成一個能真正工作的系統。**

如果你正在做 LINE Bot、企業知識庫、內部助理、客服流程或任何需要多步驟 AI 協作的產品，這份架構視角很值得直接拿去重畫一次你現在的系統圖。

很多時候，真正該重構的不是 prompt，而是整條 pipeline。
