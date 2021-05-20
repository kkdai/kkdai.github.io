---
layout: post
title: "[學習心得] LINE Bot 開發者指南詳解 -  1 關於 LINE Bot"
description: ""
category: 
- 學習文件
tags: ["LINEBot", "Chatbot", "DevRel"]
---

<img src="https://developers.line.biz/assets/img/messaging-api-architecture.f40bffbb.png">

## 前言:

這一份文件是來自於 [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/) 的投影片，考量到在台灣還沒有正式的公布與中文化。這一次跟總部共同合作準備好中文版本之外，並且特定用這一篇文章加以解釋，希望可以讓更多開發者有更多的了解。  [Development guidelines](https://developers.line.biz/en/docs/partner-docs/development-guidelines/)  文件內容很多，本篇文章也將以五篇文章的篇幅來加以解釋。



## 文章索引:

希望各位可以持續關注：

1. 關於LINE Bot (本篇文章)
2. 使用Webhook URL接收請求時的注意事項
3. 發送 API 請求時的注意事項
4. LINE Login
5.  其他相關功能



##  LINE Developers 相關資源



這邊主要提到兩個網站，分別是：

- **[LINE Developer Doc:](https://developers.line.biz/en/docs/)**
  - 負責存放所有的相關產品說明文件， API 詳細說明。
- [**LINE Developer Console:**](https://developers.line.biz/console/)
  - 



## 結論：

<a id="summary"></a>

許多開發者在開發 Web App 的時候，最麻煩的往往是使用者資料的註冊。 因為複雜的註冊流程與多一個帳號密碼往往會讓使用者卻步。 [LINE Login](https://developers.line.biz/en/docs/line-login/) 提供良好的機制可以讓許多開發者使用到國內最多人使用的 LINE 來登入，節省掉許多開發上的麻煩。 本篇文章介紹了新的登入機制: PKCE for LINE Login 。 讓登入機制符合最新的安全機制外，更可以讓使用上沒有開發後顧之憂。  並且提供[相關範例程式碼](https://github.com/kkdai/line-login-sdk-go)x，讓開發者可以無痛的導入最新的機制。



## 相關文章：
<a id="refer"></a>

-  
