---
layout: post
title: "[開發紀錄][Node.js] Feedly Classic 回不去了，我自己動手做了一個 FeedFlow（一）"
description: ""
category:
- 開發紀錄
tags: ["Node.js", "RSS", "LINE Login", "Gemini", "Firestore"]
---

![LINE 2026-07-26 12.40.19](../images/LINE 2026-07-26 12.40.19.tiff)

## 前言:

Feedly 改版之後，介面我一直不習慣，尤其是懷念舊版 Feedly Classic 那種一欄式、資訊密度高、不拖泥帶水的閱讀節奏。訂閱的來源裡有不少英文、日文、韓文的技術部落格，每次要嘛開分頁丟去翻譯、要嘛乾脆跳過，久而久之這些來源就變成已讀不回。

與其繼續將就，我花了一個週末，自己刻了一個 RSS 閱讀器：[FeedFlow](https://github.com/kkdai/rss-feed-class-webapp)。手機瀏覽器優先、深色主題、多欄位檢視模式，這部分是在向 Feedly Classic 致敬；資料存在 Google Firestore，帳號綁 LINE Login；訂閱到的非中文文章，背景會自動用 Gemini 2.5 Flash 翻成繁體中文。目前已經部署在 [Cloud Run 上面](https://feedflow-660825558664.asia-east1.run.app)，自己每天在用。

這個 repo 會持續開發下去，這篇是系列文章的第一篇，先把整個專案的骨架、幾個關鍵決策的來龍去脈記錄下來。

# TL;DR

本篇文章會依序介紹：

- <a href="#why">為什麼要自己刻一個 RSS 閱讀器</a>
- <a href="#line-login">LINE Login 在這個專案裡代表的意義</a>
- <a href="#rss-dev">RSS 閱讀器的開發過程：從單機 MVP 到多使用者雲端同步</a>
- <a href="#gemini">為什麼選 Gemini 當翻譯引擎，中間踩了哪些坑</a>
- <a href="#frontend">前端介面：四種檢視模式與行動裝置優先設計</a>
- <a href="#secret-cleanup">意外的插曲：把洩漏的密鑰從 git 歷史裡挖乾淨</a>
- <a href="#whats-next">目前進度與下一篇的方向</a>
- <a href="#summary">結論</a>
- <a href="#refer">參考連結</a>

# 為什麼要自己刻一個 RSS 閱讀器

<a id="why"></a>

市面上不缺 RSS 閱讀器，但我要的東西很具體：手機上開起來要快、介面資訊密度要高（不要每篇文章都用一張大圖佔掉整個螢幕）、然後外文來源要能就地翻譯，不用切換到別的分頁。這三個條件湊在一起，現成的服務沒有一個完全符合，尤其是「非中文文章自動翻譯」這件事，幾乎沒有閱讀器把它當一等公民做。

專案取名 FeedFlow，核心就三塊：後端一支 Express（`server.js`），負責抓 RSS、解析內容、呼叫 Gemini、讀寫 Firestore；前端是純 Vanilla JS 的 ES Modules（`app.js`、`store.js`、`api.js`、`i18n.js`），沒有套框架；資料庫用 Firestore，帳號則是 LINE Login。整個 MVP 第一版就把訂閱管理、資料夾分類、四種檢視模式、深色主題全部生出來了，後面的每一版都是在這個骨架上加東西。

# LINE Login 在這個專案裡代表的意義

<a id="line-login"></a>

![LINE 2026-07-26 12.40.51](../images/LINE 2026-07-26 12.40.51.tiff)

我之前寫過 [如何透過 Golang 開發 OAuth2 的 PKCE](https://www.evanlin.com/go-oauth-pkce/)，講的是 LINE Login 導入 PKCE 的實作細節。那時候是把 LINE Login 當研究對象在拆解協定；這次在 FeedFlow 裡，LINE Login 換了一個角色——它是整個多使用者架構能不能成立的關鍵。

FeedFlow 最早其實沒有帳號系統，資料就存在瀏覽器的 localStorage，換一台裝置訂閱就不見了。要做雲端同步，第一件事是要有一個穩定的使用者身份，而且這個身份要能在 Firestore 裡當 document 路徑的 key（`users/{userId}/...`）。比起自己刻一套帳號密碼系統，直接用 LINE Login 換來的 LINE User ID（`sub` claim）當這把 key，省掉了整套密碼、驗證信、忘記密碼流程，對一個個人專案來說划算很多。

這個決定也不是一次到位的。最早的版本其實是偷懶版：讓使用者自己貼一段 LINE UID 字串進去，後端只是拿去當 Firestore key，沒有做任何身份驗證——這樣的「登入」等於任何人都能冒充任何一個 UID。下一版才換成正規的 LINE OpenID Connect：標準的 OAuth 2.1 授權碼流程，拿到 `id_token` 之後用 LINE 的 `/oauth2/v2.1/verify` 驗證簽章，session 存在 HTTP-only cookie，而不是塞在網址參數裡到處跑。再之後又補上 `state` 跟 `nonce` 檢查，擋掉 CSRF。這個「先求有、再求對」的順序，某種程度上也反映了個人專案常見的節奏：先把功能兜起來確認方向對不對，安全性的坑等看得到需求了再回頭補。

因為主要使用情境是在 LINE 裡分享連結、在 LINE 內建瀏覽器打開，所以除了標準的 OAuth 重導向，也整合了 LIFF SDK，讓使用者如果是在 LINE App 裡開啟，可以直接用 LIFF 的 SSO 登入，不用再跳出去瀏覽器繞一圈。

# RSS 閱讀器的開發過程：從單機 MVP 到多使用者雲端同步

<a id="rss-dev"></a>

MVP 階段的功能其實跟一般 RSS 閱讀器沒什麼差異：貼網址進去，後端用 `rss-parser` 解析 feed，找不到 feed 就用 `cheerio` 去掃網頁 `<link>` 標籤做 auto-discovery；訂閱進來的來源可以分類到資料夾；文章可以標記已讀、全部標記已讀；提供重新整理拉取最新文章。這一版資料還全部存在 localStorage。

真正讓專案變得「像一個產品」的是接上 Firestore 之後的那幾版。每個使用者的訂閱清單、資料夾、閱讀進度、偏好設定，都寫進 `users/{LINE_UID}/...` 底下獨立的路徑，多租戶資料互不干擾。閱讀進度記到很細：不只是「這篇讀過了」的 ID 清單（`readArticleIds`），還記了每個 feed 目前讀到哪一篇（`lastReadArticleId`），換裝置或重新整理都接得回去。

比較有意思的是「auto-hydration」這個小機制：Cloud Run 重新部署、或是換一台裝置登入時，記憶體裡是沒有文章資料的，只有 Firestore 裡的訂閱清單。這時後端會在背景把訂閱清單裡每個 feed 重新抓一次、解析出文章，再把畫面填滿，使用者不會看到一個空的「請先訂閱」畫面，銜接得算自然。

後來又加了一版「豐富預覽」：貼 RSS 網址進去的當下，除了抓 feed 的標題跟簡介，還會順便抓最新三篇文章當樣本，非中文的內容一併丟給 Gemini 翻譯，訂閱前就能看懂這個來源在寫什麼，不用真的訂下去才發現是自己看不懂的語言。

# 為什麼選 Gemini 當翻譯引擎，中間踩了哪些坑

<a id="gemini"></a>

會需要翻譯，是因為訂閱清單裡有不少非中文來源。判斷邏輯很單純：後端偵測文章語言，不是繁體中文的，就丟給 Gemini 2.5 Flash 翻，回傳 JSON 結構的 `translatedTitle` / `translatedContent`，前端在文章卡片跟閱讀器都加一個「✨ 繁中」的翻譯徽章，閱讀器裡還有一顆按鈕可以在原文跟翻譯之間切換。選 Gemini 沒有太多懸念，2.5 Flash 的延遲跟成本都適合這種「每次進畫面就要順手翻好幾篇」的用量，用其他家 API 也做得到，但當時手上這個 GCP 專案本來就在用，順手接上去而已。

第一版是最直接的做法：拿 `GEMINI_API_KEY` 直接打 Generative Language API。能動，但部署到 Cloud Run 之後，這代表要多管理一組 API Key 環境變數，金鑰外洩的風險也跟著多一份。後來把翻譯這段改成走 Vertex AI，用 Cloud Run 服務本身的身份（ADC，Application Default Credentials）驗證，不用另外持有一把 API Key——Cloud Run 的 service account 本身就有權限呼叫 Vertex AI，設定 IAM 就好，不用在程式碼或環境變數裡塞任何密鑰。本地開發沒有 ADC 環境的時候，才退回用 `GEMINI_API_KEY` 當備援。

這一版上線後在 Cloud Run 的 log 裡整批翻譯請求都在失敗，訊息是「Neither Vertex AI ADC self-identity nor GEMINI_API_KEY is available」，明明兩條路都設定了。查下去才發現是 `@google/genai` SDK 的用法搞錯：它要的是 `vertexai: true` 這個布林值，加上 `project`、`location` 兩個平行參數；我一開始寫成巢狀的 `vertexai: { project, location }`，SDK 判斷「有沒有開 Vertex 模式」跟「有沒有讀到 project/location」是兩段分開的邏輯，結果變成 SDK 以為 Vertex 模式開著，卻讀不到專案跟區域，每次呼叫都在起手式就死掉。

與其去啃 SDK 的參數規則，最後乾脆把 `@google/genai` 整包拿掉，改用 `google-auth-library` 的 `GoogleAuth` 直接要一張 ADC token，自己組 HTTP request 打 Vertex AI 的 `generateContent` REST endpoint。少一層 SDK 包裝，行為反而更好預測——這種時候繞過 SDK、直接打 REST API，比在文件裡找 SDK 到底哪個參數該巢狀哪個該平行，省事很多。

# 前端介面：四種檢視模式與行動裝置優先設計

<a id="frontend"></a>

介面設計的參考座標就是 Feedly Classic：一個右上角的檢視模式切換鈕，四種模式都在同一份文章資料上套不同版型，不用重新抓資料。

| 模式 | 特色 |
|---|---|
| Magazine（雜誌） | 預設模式，圖文並陳的摘要卡片，適合快速掃過標題跟一小段內容 |
| List（列表） | 高密度純文字列表，一眼能看到的文章數量最多 |
| Title Only（純標題） | 只留標題，捲動速度最快 |
| Cards（卡片） | 大圖為主的視覺卡片，適合圖片內容豐富的來源 |

側邊欄是資料夾結構，每個資料夾跟未分類的訂閱各自列出，旁邊帶未讀數字徽章；文章清單跟閱讀器是兩塊面板，點文章從清單滑進閱讀器、上滑手勢滑回清單，操作邏輯照著手機原生 App 的習慣走，不是網頁常見的那種點連結整頁跳轉。整體是深色主題，topbar 放了選單、檢視模式、全部已讀、重新整理、設定幾顆圖示按鈕，登入後 LINE 顯示名稱直接顯示在原本「LINE 登入」按鈕的位置。

設定頁裡把「介面語言」跟「翻譯目標語言」拆成兩個獨立選項：介面語言目前有 zh-TW / en / ja 三種，翻譯目標語言則是決定 Gemini 要把外文翻成哪種語言，這兩件事沒有理由綁在一起，一個使用者的介面可能開英文，但還是想把日文文章翻成繁體中文看。



# 目前進度與下一篇的方向

<a id="whats-next"></a>

寫這篇的當下，repo 裡剛好新增了兩份文件（`docs/superpowers/specs/` 跟 `docs/superpowers/plans/`），在規劃文章列表的分頁瀏覽，每五篇一頁、支援滑動手勢、滾輪、按鈕三種方式翻頁。這部分還在開發中，等做完會是這個系列的下一篇。

# 結論

<a id="summary"></a>

FeedFlow 目前解決的問題很單純：手機上快速掃過一堆外文技術文章，不用切分頁翻譯、不用忍受臃腫的介面。三個決策撐起了現在這個架構——LINE Login 提供免刻帳密系統的使用者身份、Firestore 撐多使用者雲端同步、Gemini 2.5 Flash（走 Vertex AI ADC，不落地存 API Key）處理翻譯。這幾塊都不是一次做對，LINE 登入從貼字串進化到正規 OAuth，翻譯從裸 API Key 換成服務身份驗證，都是先跑通、再補強。

# 參考連結：

<a id="refer"></a>

- [如何透過 Golang 開發 OAuth2 的 PKCE - 以 LINE Login 為例](https://www.evanlin.com/go-oauth-pkce/)
- [kkdai/rss-feed-class-webapp](https://github.com/kkdai/rss-feed-class-webapp)
- [FeedFlow 線上 Demo](https://feedflow-660825558664.asia-east1.run.app)
- [LINE Login 官方文件](https://developers.line.biz/en/docs/line-login/)
- [Gemini API 文件](https://ai.google.dev/)
- [Vertex AI - Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials)
