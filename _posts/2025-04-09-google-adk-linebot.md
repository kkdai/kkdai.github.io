---
layout: post
title: "[Gemini][LINEBot] 透過 Google ADK 打造一個 Agent LINE Bot"
description: ""
category: 
- Python 
- TIL
tags: ["python", "Gemini", "Google"]
---



![image-20250410202925234](../images/2022/image-20250410202925234.png)

# 前言

雖然前幾天我才剛將 [OpenAI Agents SDK 整合成一個簡單的 LINE Bot 範例程式](https://www.evanlin.com/gemini-openai-agent-sdk/)，就在 20250410 的凌晨， Google 就宣佈了 ADK (Google Agent SDK) 的發佈。

本篇文章將介紹如何透過 Google Agent SDK (ADK) 來打造一個最簡單的 LINE Bot 功能，作為之後 MCP 與其他功能的起始專案。 (想不到才沒隔多久，就可以換成 Google ADK XD)

#### 範例程式碼：  [https://github.com/kkdai/linebot-adk](https://github.com/kkdai/linebot-adk)

## 快速簡介 Google ADK

#### Repo: [https://github.com/google/adk-python](https://github.com/google/adk-python)

![intro_components.png](../images/2022/quickstart-flow-tool.png)

Google 推出的Agent Development Kit (ADK)，這是一個開源框架，旨在簡化智能多代理系統的開發。以下是內容的重點：

- ADK是一個開源框架，專為開發多代理系統而設計，提供從構建到部署的全方位支持。
- 它支持模組化和可擴展的應用程序開發，允許多個專業代理的協作。

**功能特點**：

- **內建串流**：支持雙向音頻和視頻串流，提供自然的人機互動。
- **靈活的編排**：支持工作流代理和LLM驅動的動態路由。
- **集成開發者體驗**：提供強大的CLI和可視化Web UI，便於開發、測試和調試。
- **內建評估**：系統性地評估代理性能。
- **簡易部署**：支持容器化部署。

## 支援視覺化測試 WebUI

![adk-web-dev-ui-chat.png](../images/2022/adk-web-dev-ui-chat.png)

(Refer: [https://google.github.io/adk-docs/get-started/quickstart/#run-your-agent)](https://google.github.io/adk-docs/get-started/quickstart/#run-your-agent))

可以在本地端透過 WebUI 來做一些快速的測試，並且

### 整合 LINE Bot SDK 需要注意的事項：



## 成果與如何使用



## 快速總結與未來發展

