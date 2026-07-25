---
layout: post
title: "[學習心得][Golang] AI Agent 時代的授權難題：ID-JAG 是什麼？為什麼我用 Go 重新實作了一次"
description: ""
category:
- 學習文件
tags: ["Golang", "MCP", "OAuth2", "Athenz", "AI Agent"]
---

## 前言:

這半年多讓 AI Agent 直接接上內部系統、幫忙做事，已經不是什麼新鮮事了。但只要仔細想一步：Agent 要用「誰的身份」去呼叫那些 API？如果它拿到的權限跟人一樣大，一旦被騙去執行了不該做的操作，後果可能比人自己手滑還嚴重。

這正是 ID-JAG（Identity Assertion JWT Authorization Grant）想解的問題。我最近把這套機制的原理整理了一遍，也照著 [`athenz-community/id-jag-the-hard-way`](https://github.com/athenz-community/id-jag-the-hard-way) 這個教學 repo，把裡面的 MCP Server 用 Go 重新實作了一遍：[`kkdai/id-jag-mcp`](https://github.com/kkdai/id-jag-mcp)。這篇文章想把 ID-JAG 的技術原理講清楚：它建立在哪些 RFC 標準上、跟我之前寫過的 OAuth2 / PKCE 有什麼不一樣、實際的 token 交換流程長什麼樣，最後也會示範怎麼把這個 Go 專案跑起來、怎麼測試。

# TL;DR

本篇文章會依序介紹：

- <a href="#what-is-idjag">什麼是 ID-JAG？為什麼需要它？</a>
- <a href="#oauth-review">從 OAuth2、PKCE，到 Agent 時代的新問題</a>
- <a href="#rfc-foundation">兩塊 RFC 基石：Token Exchange 與 JWT Bearer</a>
- <a href="#idjag-flow">ID-JAG 的完整 token 交換流程</a>
- <a href="#least-privilege">每一跳都窄化權限：最小權限原則怎麼落地</a>
- <a href="#why-go">為什麼要用 Go 重新實作這個 MCP Server？</a>
- <a href="#try-it">動手玩：安裝、執行與測試</a>
- <a href="#summary">結論</a>
- <a href="#refer">參考文章</a>

# 什麼是 ID-JAG？為什麼需要它？

<a id="what-is-idjag"></a>

ID-JAG 是一種讓 AI Agent 代替使用者去存取受保護資源的授權機制，重點是「代替」這兩個字。目前它還是 IETF 的 Internet-Draft，還沒成為正式 RFC，但已經有 LY Corporation（在 Athenz 授權系統上）跟 Okta 等單位開始實作，MCP（Model Context Protocol）規範也已經引用了這份草案。

傳統的服務對服務授權，通常是兩種極端：要嘛整個服務共用一把萬用鑰匙（API Key、Service Account），要嘛乾脆把使用者的 session 或長效 token 直接借給程式用。前者權限太大，後者不僅沒有審計軌跡，一旦洩漏，攻擊者幾乎可以完全冒充使用者，而且很難被偵測出來。等到 AI Agent 開始自己決定要呼叫哪些工具、串接哪些內部 API 的時候，這兩種做法的風險都被放大了：Agent 可能因為 prompt injection 或幻覺，做出使用者根本沒打算做的操作，而如果 Agent 手上握的是一把萬用鑰匙，後果就是全公司資料任其存取。真實場景裡也常常是「Orchestrator Agent 呼叫 Sub-Agent」的多層架構，風險是一路往下傳遞的。

ID-JAG 想做到的是：Agent 每一次行動，都必須能證明「這是某個特定使用者，在某個特定當下，授權我做這一件特定的事」，而且這個授權範圍要盡可能窄、有效期要盡可能短。它建立在 OAuth2 的 token exchange（[RFC 8693](https://datatracker.ietf.org/doc/html/rfc8693)）之上，多蓋了一層「這個 token 是從人的身份主張衍生出來的」證明機制。這也是為什麼它同時被列在 [OAuth.net 的 Cross-App Access（XAA）頁面](https://oauth.net/cross-app-access/) 上——這正是 Agent 時代才會冒出來的新問題。

順帶一提，ID-JAG 也解決了一個很實際的使用體驗問題：如果每次 Agent 要存取一個新服務，都得跳出瀏覽器視窗讓使用者手動按「同意」，這種體驗很快就會讓人疲乏、乾脆什麼都按同意。ID-JAG 把授權動作收斂到使用者透過 SSO 登入的那一刻，之後 Agent 需要新的存取權限時，是拿著已經核發的身份主張去跟授權伺服器換 token，不需要使用者再跳出來按一次。

# 從 OAuth2、PKCE，到 Agent 時代的新問題

<a id="oauth-review"></a>

我之前寫過 [如何透過 Golang 開發 OAuth2 的 PKCE](https://www.evanlin.com/go-oauth-pkce/)，內容是 LINE Login 導入 PKCE 的實作經驗。那篇文章解的問題，跟 ID-JAG 解的問題其實完全是兩個層次，拿出來對比一下會更清楚 ID-JAG 到底新在哪裡。

PKCE 解的是「client 身份是否可信」的問題：手機 App 這種沒辦法安全保管 client secret 的公開客戶端，authorization code 有可能在傳遞過程中被同一支手機上的惡意 App 攔截。PKCE 用 `code_verifier` / `code_challenge` 這組一次性配對，確保就算 code 被偷了，沒有正確的 verifier 也換不到 token。整個問題發生在「使用者本人跟他手上那支 App」之間的單一跳（single hop）。

ID-JAG 解的則是「這個非人類的服務身份，有沒有資格代表這個人做這件事」的問題，而且往往橫跨好幾跳：使用者登入 IdP → AI Client Gateway → MCP Server → 最終的 Resource Server。每一跳的呼叫者都不是使用者本人，卻都得證明自己是「奉旨行事」。傳統 OAuth 2.0 一開始的設計，是為「人類使用者 ↔ 應用程式」這種場景設計的，並不直接支援這種多層 Agent 鏈的委派情境。PKCE 保護的是一次授權交換的完整性；ID-JAG 保護的是一整條授權鏈路上，每一個環節的最小必要權限。兩者不衝突，是同一個大架構下，解決不同階段問題的機制。

# 兩塊 RFC 基石：Token Exchange 與 JWT Bearer

<a id="rfc-foundation"></a>

ID-JAG 沒有憑空發明一套新協定，而是把兩個既有的 IETF 標準組合起來用。搞懂這兩塊基石，才看得懂後面完整的交換流程在做什麼。

第一塊是 RFC 8693 — OAuth 2.0 Token Exchange，定義了一個通用的「用一種 token 換另一種 token」協定，概念上有點像去外幣兌換所把日圓換成台幣，只是這裡換的是安全令牌。一個 token exchange 請求大致長這樣：

```
POST /token
grant_type=urn:ietf:params:oauth:grant-type:token-exchange
subject_token=<身份主張 JWT>
subject_token_type=urn:ietf:params:oauth:token-type:jwt
requested_token_type=urn:ietf:params:oauth:token-type:access_token
scope=read:orders
```

`subject_token` 放的是代表被委派身份的 token，`requested_token_type` 說明想換成什麼型別的 token，`scope` 則可以在交換的當下就把權限範圍縮小。

第二塊是 RFC 7523 — JWT Bearer Grant，讓一個 JWT 本身就可以直接拿來當 OAuth 2.0 的授權憑證，不用先繞去換一次 authorization code：

```
POST /token
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
assertion=<已簽名的身份主張 JWT>
```

授權伺服器收到之後，用簽發者（IdP）的公鑰驗證這個 JWT 的簽章，確認 `audience`、`scope`、`exp` 這些欄位都合理，就可以直接核發 Access Token，不需要使用者二次同意——因為 IdP 已經替這個身份主張背書過了。

這兩者要交換的「身份主張 JWT」，一般會包含以下欄位：

```json
{
  "iss": "https://enterprise.idp.example.com/v1",
  "sub": "alice@company.com",
  "aud": "https://api.service.example.com",
  "exp": 1773839486,
  "iat": 1773825086,
  "jti": "abc123-7dc6-42ab-b326-uniqueid",
  "scope": "read:orders write:tickets"
}
```

`sub` 是被代理的使用者、`aud` 是這份主張要交給哪個服務用、`scope` 是允許的權限範圍、`jti` 則是唯一 ID，用來防止同一份主張被重放使用。ID-JAG 就是把這份短效的身份主張，透過上面兩個 RFC 定義的交換機制，逐步換成 Agent 真正能拿去呼叫 API 的 Access Token。

# ID-JAG 的完整 token 交換流程

<a id="idjag-flow"></a>

以 `id-jag-the-hard-way` 這個教學 repo 的架構為例（用 Athenz 當授權伺服器），完整的鏈路是把前面兩個 RFC 串接起來使用：

```
使用者登入 IdP（Keycloak）
   │  拿到 OIDC ID Token
   ▼
AI Client Gateway
   │  用 RFC 8693 Token Exchange，把 ID Token 換成 ID-JAG
   │  （grant_type=token-exchange, subject_token_type=id_token,
   │   requested_token_type=id-jag）
   ▼
再用 RFC 7523 JWT Bearer，拿 ID-JAG 去換一個真正能用的 Athenz Access Token
   │  （grant_type=jwt-bearer, assertion=<ID-JAG>）
   ▼
AI Client 帶著這個 Access Token 呼叫 MCP Server
   │
   ▼
MCP Server 收到請求後，「自己」再做一次 RFC 8693 Token Exchange
   │  把收到的 Access Token 換成「這個工具需要的最小 scope」的新 Access Token
   │  （這一步用的是 MCP Server 自己的 mTLS service identity，不是使用者的憑證）
   ▼
用換到的窄化 Access Token 呼叫最終的 Resource Server
```

整條鏈路裡，token 被交換了不只一次，而是每經過一個信任邊界就交換一次，範圍也越換越窄。這個設計刻意讓每一段路徑的「憑證」都不一樣——AI Client Gateway 手上的 token 沒辦法直接拿去騙過最終的 Resource Server，因為 MCP Server 這一關會強制重新驗證、重新換發。核發出來的 Access Token 裡，通常也會同時記錄 `sub`（被代理的使用者）跟 `act`（實際執行操作的 Agent 身份）兩個欄位，下游服務因此可以清楚看到「Alice 透過某個 Agent 執行了這個操作」，稽核記錄不會斷在半路。

# 每一跳都窄化權限：最小權限原則怎麼落地

<a id="least-privilege"></a>

這是我覺得整個架構設計得最漂亮的地方：最小權限不是寫在文件裡的原則，而是被 token exchange 這個機制物理性地強制執行。

以我重新實作的 `id-jag-mcp` 為例，它對外提供三個工具：

| 工具 | 對應的 Athenz Scope |
|---|---|
| `get_k8s_docs` | `api:role.docs-getter` |
| `delete_k8s_doc` | `api:role.docs-deleter` |
| `post_k8s_doc` | `api:role.docs-poster` |

MCP Server 收到請求時，不會直接把 AI Client 傳來的 Access Token 轉發給上游 API——它一定會先用自己的 mTLS 憑證，向 Athenz ZTS 拿著「這個工具實際需要的 scope」重新交換一次 token，換到的新 token 才會拿去呼叫上游。就算 AI Client Gateway 那邊的 token 範圍比較寬（例如同時擁有讀跟刪的權限），MCP Server 在轉發之前，也只會替每一個工具要求它真正需要的那一小塊權限。

換句話說，整個系統裡沒有任何一個環節「順手」擁有比它當下任務更大的權限——這不是靠 code review 或是內規檢查出來的，而是架構上根本沒辦法繞過。

# 為什麼要用 Go 重新實作這個 MCP Server？

<a id="why-go"></a>

`id-jag-the-hard-way` 原本的 MCP Server（`api_server/mcp/`）是用 TypeScript + Express 寫的，而且是手刻 JSON-RPC 2.0 協定（沒有用官方 SDK）。我想確認兩件事：第一，這套 token 交換架構如果換一個語言、換一個 MCP SDK 實作，邏輯是不是真的完全可以複製；第二，官方的 [`modelcontextprotocol/go-sdk`](https://github.com/modelcontextprotocol/go-sdk) 用起來到底如何。

最後的實作維持了原本的核心邏輯（同樣的 scope 對應、同樣的 mTLS token exchange 流程），但把協定層整個換成官方 Go SDK，mTLS client 則是自己刻的（沒有依賴 Athenz 官方的 Go client library）。

# 動手玩：安裝、執行與測試

<a id="try-it"></a>

程式碼在 [`kkdai/id-jag-mcp`](https://github.com/kkdai/id-jag-mcp)（Apache 2.0 授權）。專案結構跟職責切分大致是：

```
cmd/id-jag-mcp/       進入點：讀取設定、組裝所有元件、啟動 HTTP server
internal/config/      環境變數設定讀取
internal/athenz/      mTLS client + Athenz ZTS RFC 8693 token exchange
internal/tools/       工具輸入型別 + 共用的上游轉發邏輯
internal/server/      MCP 工具註冊（官方 SDK）+ REST 快捷路由 + logging
```

先把專案抓下來、建置成 binary：

```sh
git clone https://github.com/kkdai/id-jag-mcp.git
cd id-jag-mcp
go build -o id-jag-mcp ./cmd/id-jag-mcp
```

要實際跑起來，需要準備 mTLS 憑證、以及一個可連線的 Athenz ZTS 和上游 API server，設定完全透過環境變數：

```sh
mkdir -p certs
cp /path/to/api-mcp.crt /path/to/api-mcp.key /path/to/ca.crt certs/

export UPSTREAM_BASE_URL=http://localhost:14443
export AUTHORIZATION_SERVER_URL=https://athenz-zts-server.athenz:4443/zts/v1

go run ./cmd/id-jag-mcp
```

啟動後除了 `/mcp` 這個給 MCP client 連線的 endpoint，也提供了跟三個工具對應的 REST 快捷路由，方便用 `curl` 直接測試，不需要透過 MCP client：

```sh
curl -H "Authorization: Bearer $AT" http://localhost:8101/api/docs

curl -X DELETE -H "Authorization: Bearer $AT" http://localhost:8101/api/docs/5

curl -X POST -H "Authorization: Bearer $AT" -H "Content-Type: application/json" \
  -d '{"name":"doc1","content":"hello"}' \
  http://localhost:8101/api/docs
```

如果只是想確認邏輯本身有沒有寫對，不需要真的架一套 Athenz/Keycloak 環境——測試全程用 `httptest` 模擬 ZTS 跟上游 API：

```sh
go build ./...
go vet ./...
go test ./...
```

例如 `internal/athenz` 的測試會起一個假的 ZTS server，驗證送出去的 `grant_type`、`subject_token`、`scope`、`audience` 這些參數是否正確；`internal/tools` 的測試則會驗證每個工具轉發給上游時，帶的是「交換後窄化的 token」而不是原始收到的那個。這樣不需要真的連上 Athenz，也能確認整條 token 交換邏輯是對的。

README 裡（中英文都有）有完整的環境變數清單跟更多細節。

# 結論

<a id="summary"></a>

ID-JAG 解的不是「這個 client 是不是它自己說的那個 client」（這是 PKCE 的問題），而是「這個非人類的服務身份，有沒有資格代表某個特定的人，在當下做這一件特定的事」。撐起這套架構的不是文件上的約束，是把 RFC 8693 跟 RFC 7523 這兩個標準串成一條鏈，讓每一跳都被強制重新驗證、重新窄化權限。

如果你的 AI Agent 已經開始接觸內部系統，這是很值得花時間搞懂的一套架構——而且不一定要照抄 Athenz 這一套，重點是理解「每一跳都要重新換發、範圍要越換越窄」這個核心原則，套用到自己的授權伺服器上。

# 相關文章：

<a id="refer"></a>

- [如何透過 Golang 開發 OAuth2 的 PKCE - 以 LINE Login 為例](https://www.evanlin.com/go-oauth-pkce/)
- [athenz-community/id-jag-the-hard-way](https://github.com/athenz-community/id-jag-the-hard-way)
- [kkdai/id-jag-mcp](https://github.com/kkdai/id-jag-mcp)
- [OAuth.net - Cross-App Access (XAA)](https://oauth.net/cross-app-access/)
- [RFC 8693 - OAuth 2.0 Token Exchange](https://datatracker.ietf.org/doc/html/rfc8693)
- [RFC 7523 - JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://datatracker.ietf.org/doc/html/rfc7523)
- [RFC 7636 - Proof Key for Code Exchange by OAuth Public Clients](https://tools.ietf.org/html/rfc7636)
- [modelcontextprotocol/go-sdk](https://github.com/modelcontextprotocol/go-sdk)
- [Athenz 官方文件](https://athenz.github.io/athenz/)
- [ID-JAG IETF Internet-Draft](https://datatracker.ietf.org/doc/draft-ietf-oauth-identity-assertion-authz-grant/)
