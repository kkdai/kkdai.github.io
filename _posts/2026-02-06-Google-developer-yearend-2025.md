---
layout: post
title: "[Recap] Google Developer Year-end 2025：Gemini 2025 新功能回顧與 LINE Bot 的完美結合"
description: "深入 AP2 協議的 Credential Provider 層，實作安全的支付憑證管理與 Token 化機制"
category:
- 研討會心得
tags: ["Google", "Gemini", "LINEBot", "AI", "VibeCoding", "DevRel"]
---

![image-20260207202439911](../images/image-20260207202439911.png)s

## 前情提要

  昨天參加了 Google 舉辦的開發者尾牙聚會（Google Developer Year-end 2025），也順便參觀了 Google 板橋辦公室。很開心能以 LINE Taiwan Developer Relations 的身份，跟大家分享我在 2025 年一整年觀察到的 Gemini 技術演進心得。

  在熱門動畫《葬送的芙莉蓮》中，我很喜歡一級魔法使測驗篇的角色「尤蓓爾」。她有一個獨特的能力概念：「只要能想像切得開，就一定能切得開」。

  這句話完美呼應了現在的 AI 時代——**想像力與理解力變得比以往更加重要**。如何「精準地想像出解決問題的方式」，成為了讓 AI 能精準協助你的關鍵。這篇文章將整理當天分享的 Gemini 2025 重點功能，以及我對「軟體工程師」在 AI 浪潮下核心能力的看法。

  ### 2025 Gemini 功能演進回顧

  回顧 2025 年，Gemini 與 LINE Bot 的結合在多個時間點都有突破性的更新。以下是重新審視這一年來的技術節點：

  | 時間點      | 功能更新            | 說明                                                         |
  | ----------- | ------------------- | ------------------------------------------------------------ |
  | **2025.04** | Google ADK          | Agent 與 Messaging API 的初步結合，展示了天氣查詢等基礎 Agent 應用。 |
  | **2025.06** | Gemini CLI          | 開發者體驗大升級，直接在終端機與 AI 協作，進行檔案操作與程式撰寫。 |
  | **2025.08** | Video Understanding | 支援 YouTube 影片理解。透過 Gemini 2.5 直接抓取字幕與影像內容進行摘要與互動。 |
  | **2025.11** | File Search         | 強化檔案搜尋能力，支援 JSON、JS、PDF、Python 等多種格式的 RAG 應用。 |
  | **2025.12** | Map Grounding       | 結合 Google Maps Platform，讓 Bot 能回答「最近的地震資訊」或「附近的餐廳」等地理資訊問題。 |

  #### 技術亮點詳解

  ##### 1. Gemini CLI 與 Vibe Coding

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/d2de8583542d473baeda52a5b212eec6?slide=4" title="Gemini 2025 新功能回顧 LINE Bot 完美結合" allowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 100%; height: auto; aspect-ratio: 560 / 315;" data-ratio="1.7777777777777777"></iframe>

  在六月推出的 Gemini CLI 改變了許多開發者的習慣。不僅僅是印出 "Hello World"，它整合了 Git、Gcloud 等工具。這帶出了一個新的開發概念：**Vibe Coding**。

  - **定義**：這不僅是寫程式，而是透過 Gemini CLI、Vertex AI Studio 和 Antigravity 等工具，讓開發流程進入一種「心流」狀態。
  - **關鍵**：重點在於開發者如何指揮這些工具串接，而非手刻每一行代碼。

  ##### 2. 視覺與地理資訊的整合 (Video & Map)

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/d2de8583542d473baeda52a5b212eec6?slide=7" title="Gemini 2025 新功能回顧 LINE Bot 完美結合" allowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 100%; height: auto; aspect-ratio: 560 / 315;" data-ratio="1.7777777777777777"></iframe>

  八月的 Video Understanding 讓我們能直接丟入 YouTube 連結，Gemini 就能生成摘要甚至回答影片細節。到了年底的 Map Grounding，則是補足了 LLM 最缺乏的「實時地理資訊」。

  - **應用場景**：用戶問「找餐廳」，Bot 透過 Map Grounding 找出附近的 "CHILLAX" 或 "博感情" 等餐廳，並附上地址與類型。
  - **資料來源**：結合了 World Knowledge (Google Search) 與 Private Knowledge (Your Data/RAG)，讓回答更具落地性。

  ### 重新審視：卓越人才的三大支柱

<iframe class="speakerdeck-iframe" frameborder="0" src="https://speakerdeck.com/player/d2de8583542d473baeda52a5b212eec6?slide=11" title="Gemini 2025 新功能回顧 LINE Bot 完美結合" allowfullscreen="true" style="border: 0px; background: padding-box padding-box rgba(0, 0, 0, 0.1); margin: 0px; padding: 0px; border-radius: 6px; box-shadow: rgba(0, 0, 0, 0.2) 0px 5px 40px; width: 100%; height: auto; aspect-ratio: 560 / 315;" data-ratio="1.7777777777777777"></iframe>

  在技術工具日新月異的同時，我也在思考，什麼樣的能力是 AI 無法取代的？就像前面提到的「尤蓓爾」的想像力，我認為卓越人才需要具備三大支柱：

  #### 1. AI 協作力 (AI Collaboration)

  這不僅僅是會用工具，而是具備 **Prompt Engineering** 的能力。

  - **差異**：懂得如何與 AI 對話、引導 AI 產出結果的人，生產速度可以提升 10 倍。
  - **重點**：AI 是你的 Copilot，但你是機長。

  #### 2. 專業深度 (Domain Depth)

  在 AI 氾濫的時代，**Domain Knowledge（領域知識）** 是你最強的護城河。

  - **價值**：AI 可以寫出語法正確的程式碼，但「解決複雜問題的經驗」與「對業務邏輯的深刻理解」，是 AI 難以模仿的。

  #### 3. 同理與創意 (Empathy & Creativity)

  將「熱情」轉化為人類特有的 **批判性思考** 與 **同理心**。

  - **核心**：這是建立人脈與管理決策的核心。AI 可以處理數據，但無法理解人的情感與需求背後的真正動機。



  ### 結語

  這次的 Google Developer Year-end 讓我再次確認，系統的理解與對問題的認知，是軟體工程師最重要的能力。

  AI 時代下的開發，不再只是單純的 Coding，而是像設計 AP2 協議一樣，需要考慮整體的架構與安全性。若我們只是快速地 Vibe Coding 而忽略了底層原理（例如忘記考慮 Token 的時效性或資料的正確性），很容易就會產出有漏洞的系統。

  所以，保持對技術的好奇心，同時深化自己的領域知識，這才是我們證明 AI 無法取代我們的重要原因。

  > **投影片下載**： 有興趣的朋友可以參考當天的投影片： [https://speakerdeck.com/line_developers_tw/2025-features-recap-perfect-integration-linebot](https://speakerdeck.com/line_developers_tw/2025-features-recap-perfect-integration-linebot)

  *(如果覺得有幫助，歡迎分享這篇文章！)*
