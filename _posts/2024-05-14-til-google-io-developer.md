---
layout: post
title: "[Google I/O] Google Developer I/O Developer Keynote 整理"
description: ""
category: 
- Golang
- TIL
tags: ["Google", "GDE"]

---

![image-20240515122008130](../images/2022/image-20240515122008130.png)

主要觀賞 link: [https://www.youtube.com/watch?v=ddcZnW1HKUY](https://www.youtube.com/watch?v=ddcZnW1HKUY)

## AI 摘要整理：

📚 整體摘要

- 此摘要涵蓋了在會議中如何使用 Gemini API 和 Google 的 AI 工具以及相關技術來強化應用程式開發的介紹。

🔖 重點概念

- Gemini API 可以整合至 Android Studio 和其他開發工具，增強開發效率。
- 透過使用 Google Cloud 和 Vertex AI，開發人員能夠接觸到更強大的 Gemini 功能。
- 提供多平台開發的一體化工具，如 Flutter 與 Firebase，用以支持快速開發。
- 開發者可以自定義 AI 模型，改善應用性能與用戶體驗。
- 強調隱私和安全，在開發過程中保護用戶數據與合規性。

💡 為什麼我們要學這個？

- 理解和運用現代 AI 和多平台整合工具是提升開發效率和應用創新性的關鍵。

❓ 延伸小問題

- 在你的下一個開發項目中，你可能如何利用 Gemini API 或其他 Google AI 工具來提升產品的性能或用戶體驗？



# 幾個重點

## Gemini 1.5 Flash

<img src="../images/2022/image-20240515091748133.png" alt="image-20240515091748133" style="zoom:25%;" />

- 支援兩百個國家以上

<img src="../images/2022/image-20240515091903439.png" alt="image-20240515091903439" style="zoom:25%;" />

它擁有以下主要功能與特典：

1. 快速反應：Gemini 1.5 Flash 能夠生成更快的回應，進而提高使用效率。 
2. 適應性：它能夠對翻譯、推理和編碼等任務的能力進行改善，使其更加強大。 
3. 增強的理解能力：這一模型擁有 100 萬個 token 的上下文窗口，進而提高其運行的效

### Google AI Sudio 支援 2M token context windows

- 原本 Gemini 1.5 Pro 是 1M token

### Google Gemini API Competition

- <img src="../images/2022/image-20240515093249577.png" alt="image-20240515093249577" style="zoom:25%;" />

- [https://ai.google.dev/competition?hl=zh-tw](https://ai.google.dev/competition?hl=zh-tw)

- <img src="../images/2022/image-20240515093252124.png" alt="image-20240515093252124" style="zoom:25%;" />
- Google AI Competition 贏得人可以拿到 DoLorean (回到未來那一台)
- <img src="../images/2022/image-20240515093942157.png" alt="image-20240515093942157" style="zoom:25%;" />
- 甚至還找了「回到未來」的飾演博士的演員（真的也太老開發者才會知道的）

## Gemini on Android - Gemini Nano

<img src="../images/2022/image-20240515102730901.png" alt="image-20240515102730901" style="zoom:25%;" />

- **OS 中似乎有個 AI Core**
  - Gemini Nano 和 LoRA 在 Android 的 AI Core 中主要是為了提升 Android 設備的本地 AI 能力。這種設定使得設備能夠在不依賴雲端服務的情況下有效地執行 AI 強化的任務，進而提升隱私、減少延遲和降低數據使用。 
  - Gemini Nano 是 Google 專為本地任務優化的 Gemini AI 模型。它設計用於直接在移動設備處理器上運行，能夠支援多種重要用例，如摘要、提問回答、實體抽取和校對。這使得應用程式能夠在裝置離線的情況下提供高品質的反應，並具有對話意識。 
  - LoRA（Low-Rank Adaptation）是一種允許在不大幅增加模型尺寸的情況下有效地微調大型語言模型的技術。在 Android 的 AI Core 中，LoRA 可以被用來將 Gemini Nano 模型適應特定任務或領域，使其更加高效和針對個別應用程式的需求。 
  - 通過在 Android 的 AI Core 中加入 Gemini Nano 和 LoRA，Google 旨在為開發者提供強大的工具，以便構建 AI 強化的功能和應用程式。這將為使用者在各種任務和領域提供更智能、更敏捷、更保護隱私的體驗。

<img src="../images/2022/image-20240515103328968.png" alt="image-20240515103328968" style="zoom:25%;" />

(GPT4-Ｏ 本來以為 Grammarly 要死～結果 Andorid 內建)

- 原來三星之前就有，Google 只是整個收進 Android OS https://twitter.com/Chiebuniem_/status/1493926483500412931/photo/1

放入 Android OS 中能做到：

1. 拼字檢查：即時發現拼字錯誤，提醒使用者對錯誤的單字進行更正。 
2. 文法檢查：幫助使用者找出文法錯誤並提供正確的用法建議。 
3. 句子結構：協助使用者改善句子結構，提升句子的清晰度和流暢度。 
4. 清晰度和簡潔性：提供建議以提升寫作的清晰度和簡潔性。 
5. 語氣和友善度：協助使用者適當調整語氣和友善度，以符合不同情境的需求。 
6. 個人字典：允許使用者添加自己的單字，避免將這些單字誤認為拼字錯誤。 
7. 對不同語言的支援：允許使用者選擇英式英語或美式英語的拼寫規則。 

 這些功能使 Grammarly 成為 Android 系統上的有效寫作輔助工具，幫助使用者改善寫作質量，提升溝通效率和准確性。

## Kotlin/ Gemini on Android Studio (跳過) XD

## Firebase 相關（這些蠻有趣的)

- 🐘Data Connect, PostgreSQL backend-as-a-service
- 🌎App Hosting, web hosting for modern frameworks
- ✨Genkit, a GenAI framework for app developers

