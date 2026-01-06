---
layout: post
title: "[TIL]Manus (後來被 Meta 收購) 首席科學家 季逸超 的三小時訪談"
description: ""
category:
- TIL
tags: ["Python"]

---

![image-20260106141440101](../images/image-20260106141440101.png)

(完整[影片](https://www.youtube.com/watch?v=UqMtkgQe-kI ))


## 前言

這一篇 Manus (後來被 Meta 收購) 首席科學家 季逸超 的三小時訪談，真的是許多人 AI  產業的人都不能錯過的好影片。

我應該會逐漸將我看到很棒的，放在留言中。先快速講一下我看到很值得關注與分享的地方。


身為連續 AI 創業家，季逸超 分享前十年在 AI 領域創業的部分。從 Tokenize 到 LSTM 到 Transformer 相關的應用。到兩次做 AI 瀏覽器的經驗。


接下來有談到為什麼 Manus 會成功，裡面有很多他們如何解決 Cloud Provider 與 Model Provider 無法做到的事情 - 一個好的工具庫讓 LLM 可以自動去規劃事情。

也有提到 AI Agent 很像製造業，因為有太多需要優化的部分。對於 Manus 產品規劃與方向上，他也認為做對的事情就是：「決定了什麼不去做」。


裡面有許多很快帶過去的技術概念，其實都很深。我一一寫在留言。



## 關於 MCP 使用

快速 link  [https://youtu.be/UqMtkgQe-kI?t=10342](https://youtu.be/UqMtkgQe-kI?t=10342)

關於 MCP 使用，其實 Manus 是比較保守的，`因為 MCP 這種動態發現工具的方式，會污染 Action Space ，會讓緩存命中率下降。 下降的緩存命中率，會讓成本暴增。`改善方式：

- 不在原生 Action Space 內的 MCP 調用方式

他也說這被 Anthorpic 寫成部落格，在這裡:
**[Code execution with MCP: Building more efficient agents](https://www.anthropic.com/engineering/code-execution-with-mcp)**如何利用代碼執行環境來提高使用MCP（Model Context Protocol）連接AI代理與外部系統的效率。MCP是一個開放標準，旨在解決AI代理與工具和數據的連接問題。文章指出，隨著MCP的廣泛應用，直接工具調用會導致上下文窗口過載和中間結果消耗過多的token。透過將MCP伺服器呈現為代碼API，代理可以更有效地管理上下文，減少token使用，並提高效率。
重要觀點

- **MCP的挑戰**：
- 工具定義過載上下文窗口。
- 中間工具結果消耗額外的token。
- **代碼執行的優勢**：
- 代理可以按需加載工具，並在執行環境中處理數據。
- 減少token使用，降低成本和延遲。
- 提供隱私保護和狀態管理的好處。
- **代碼執行的實現**：
- 使用TypeScript生成可用工具的文件樹。
- 代理通過文件系統探索工具，僅加載所需的定義。
- **代碼執行的其他好處**：
- 進行數據過濾和轉換。
- 使用熟悉的代碼模式進行控制流。
- 保護隱私的操作。
- 狀態持久化和技能的保存。

解決方案

- **代碼執行環境**：
- 將MCP伺服器作為代碼API。
- 代理在執行環境中運行代碼，減少上下文窗口的負擔。

注意事項

- **安全性和基礎設施要求**：
- 需要安全的執行環境，適當的沙箱、資源限制和監控。
- 這些基礎設施需求增加了操作開銷和安全考量。



## 提到 OpenAI 5 階層的 Agent



[https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf)

- Level 1: Conversational AI/Chatbots
- Level 2: Human-Level Problem Solving/Reasoners
- Level 3: Agents
- Level 4: Innovators
- Level 5: Organizers

from [https://youtu.be/UqMtkgQe-kI?t=9650](https://youtu.be/UqMtkgQe-kI?t=9650)



## 心目中影響 AI 進程的論文

當問到，你心目中影響 AI 進程的幾篇論文:

- FLAN-T5 ([Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416))
  - 透過微調參數，讓 11B 的 FLAN-T5 也有不錯的效果。
- Word2Vec 知名論文 ([Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/abs/1301.3781))
  - Word2vec 算法，透過高效的數學架構讓電腦能將文字轉化為具備語義關聯的向量，正式開啟了深度學習在自然語言處理領域的黃金時代。





## 跟創業有關的討論

另外是跟創業很有關係:
https://youtu.be/UqMtkgQe-kI?t=3783

-  當一群不太笨的人，無所事事的時候，就會有很好的想法
-  "For every complex problem there is an answer that is clear, simple, and wrong." - H. L. Mencken
