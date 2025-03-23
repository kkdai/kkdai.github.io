---
layout: post
title: "[Gemini] 在 Cline 上面使用 Gemini 來呼叫 MCP 的功能"
description: ""
category: 
- TIL
tags: ["MCP", "Gemini"]

---





## 前情提要

最近 MCP 是非常熱門的討論話題，但是大家提到 MCP 不免會想到 Anthropic 的 Claude 或是其他語言的模型。這一篇文章要告訴大家關於 MCP 的一些基礎原理，並且如何使用 Google Gemini 來呼叫 MCP 。希望能給大家一些整理。



## 什麼是 *MCP*（Model Context Protocol）

[根據 Anthropic 的文件上面有提到](https://docs.anthropic.com/zh-TW/docs/agents-and-tools/mcp): MCP 是一個開放協議，用於標準化應用程式如何為大型語言模型（LLM）提供上下文。您可以將 MCP 想像成 AI 應用程式的 USB-C 接口。就像 USB-C 為您的設備提供了一個標準化的方式來連接各種外圍設備和配件一樣，MCP 為 AI 模型提供了一個標準化的方式來連接不同的數據源和工具。

這邊也分享 YT [https://www.youtube.com/watch?v=McNRkd5CxFY&t=17s]( https://www.youtube.com/watch?v=McNRkd5CxFY&t=17s) 上面的架構圖，讓大家更容易理解

![Google Chrome 2025-03-23 16.41.43](../images/2022/Google Chrome 2025-03-23 16.41.43.png)

（圖片來源 [技术爬爬虾  TechShrimp](https://www.youtube.com/@Tech_Shrimp) :[MCP是啥？技术原理是什么？一个视频搞懂MCP的一切。Windows系统配置MCP，Cursor,Cline 使用MCP](https://www.youtube.com/watch?v=McNRkd5CxFY&t=17s)）

這邊可以看出來， 透過 MCP 這邊提到的 AI 客戶端（大家常使用的 ChatGPT, Claude, Gemini 的等等）都可以透過 MCP 架構直接來對這些服務做「操作」。

### MCP 服務中的架構圖

![image-20250323164924676](../images/2022/image-20250323164924676.png)

(架構圖：  [MCP Core architecture](https://modelcontextprotocol.io/docs/concepts/architecture))

這個架構圖，有清楚的敘述出關於 MCP 的 Client Server 的架構，這邊再強調一下。

- **MCP Host**: 使用這些 MCP 服務的應用，可能是 Cline, Windsurf 或是 Claude Desktop)
- MCP Server: 
- **MCP Client**:  在每一個 Host 中，確定使用某個 MCP Server



## 參考資料：

- [MCP Core architecture](https://modelcontextprotocol.io/docs/concepts/architecture)
- [技术爬爬虾  TechShrimp](https://www.youtube.com/@Tech_Shrimp) :[MCP是啥？技术原理是什么？一个视频搞懂MCP的一切。Windows系统配置MCP，Cursor,Cline 使用MCP](https://www.youtube.com/watch?v=McNRkd5CxFY&t=17s)
- [高見龍: MCP 是什麼？可以吃嗎？](https://www.youtube.com/watch?v=cdBRAVYZKFo)

