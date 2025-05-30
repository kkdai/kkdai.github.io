---
layout: post
title: "[Gemini][LINEBot] 輕鬆升級！從 Function Call 轉換為 Agent 模式的 ADK 實作指南"
description: ""
category: 
- Python 
- TIL
tags: ["python", "Gemini", "Google"]
---



# 前言

之前的文章曾經有分享過如何透過 Google ADK (Agent SDK) 來將你的 LINE 官方帳號 (俗稱： LINE Bot ) 打造成來。 但是其實在 LLM LINE Bot 上，我們有學過不少的 LLM 方式打造。本篇文章，將討論如何將 Function Calling 的 Agent 模式，直接改造成使用 Agent SDK 的方式。

你會發現這樣的修改，程式碼可以變得更精簡。而且由於導入了 Agent SDK ，整個對話也變得更加的靈活，更可以像是真人的對話。

## 本次的程式碼

本次將
