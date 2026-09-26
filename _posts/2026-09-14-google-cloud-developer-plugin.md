---
layout: post
title: "實測 Google 官方 google-cloud-developer plugin：讓 AI Agent 不再亂掰 gcloud 指令"
description: "實測 Google 官方 google-cloud-developer plugin：讓 AI Agent 不再亂掰 gcloud 指令"
category:
- Google Cloud
- AI
tags: ["GoogleCloud", "ClaudeCode", "Antigravity", "MCP", "gcloud"]
---

最近翻 Google Cloud 的[開發環境設定文件](https://docs.cloud.google.com/docs/get-started/developer-environment?hl=en#antigravity-cli)，
在一堆 gcloud CLI、Cloud Shell、Cloud Workstations 中間，多了一段以前沒有的東西：

```bash
agy plugin install https://github.com/google/skills/plugins/cloud/google-cloud-developer
```

`agy` 是 Google Antigravity CLI。但真正讓我停下來的不是這個新 CLI，而是後面那串網址：
Google 開始把自家的產品知識，包成 agent plugin 放在 GitHub 上發佈。

結果我打開自己的 Claude Code 一看，這個 plugin 早就裝在機器上了。所以這篇不用先裝 Antigravity CLI，
直接拿現成的來實測，看看它到底做了什麼。

### 這個 plugin 裡面裝了什麼

先把它拆開看。整包其實不大，5 個 skill、1 份路由規則、1 個 MCP server，全部加起來 1549 行 markdown，沒有任何程式碼：

```
skills/gcloud/                        # gcloud 指令的安全護欄與語法驗證（272 行 + 340 行參考文件）
skills/retrieving-developer-knowledge/ # 查官方文件（103 行 + 182 行參考文件）
skills/finding-google-skills/          # 按需從遠端目錄撈技能（134 行）
skills/google-cloud-recipe-onboarding/ # 新手開第一個專案（228 行）
skills/google-cloud-recipe-auth/       # 憑證與 ADC 選擇（260 行）
rules/google-cloud-discovery.md        # 技能路由表（30 行）
```

有趣的是同一個目錄底下塞了四套 manifest：

```
.claude-plugin/plugin.json   # Claude Code
.codex-plugin/plugin.json    # Codex
gemini-extension.json        # Gemini CLI / Antigravity
plugin.json                  # 通用格式
```

那份通用的 `plugin.json` 開頭是這樣：

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "google-cloud-developer",
  "version": "1.1.2",
  "author": { "name": "Google LLC" },
  "license": "Apache-2.0"
}
```

`agent-plugins.org` 是個跨工具的 plugin schema。Google 沒有為自己的 Antigravity 做一份專屬格式，
而是同一包東西讓四種 agent 都吃得下去。換句話說，你不裝 Antigravity CLI 也用得到這包東西。

### 安裝：三條路挑一條

**Antigravity CLI（文件上寫的）**

```bash
agy plugin install https://github.com/google/skills/plugins/cloud/google-cloud-developer
```

**Claude Code**

```bash
claude plugin marketplace add google/skills
claude plugin install google-cloud-developer@google-plugins
```

**通用**

```bash
npx skills add google/skills
```

裝完後要開 API：

```bash
gcloud services enable developerknowledge.googleapis.com --project=YOUR_PROJECT_ID
```

確認裝到哪：

```bash
$ ls ~/.claude/plugins/cache/google-plugins/google-cloud-developer/
1.1.2
```

### 實測一：護欄擋不擋得住亂掰的 flag

這個 plugin 最核心的是 `gcloud` 這個 skill。它開頭第一段話講得很直白：

> All pre-existing knowledge of `gcloud` commands, flags, flag values, and positional
> argument syntax is **stale and prone to hallucination**.

翻成白話：你（模型）記得的 gcloud 語法都是過期的，不准憑記憶寫。
所以它訂了一條硬規則：動手之前一定要先跑 `gcloud help <leaf command>`，而且父層的 help 不算數，
要驗到最底層的子指令。它甚至明文禁止用網路搜尋來查語法，`gcloud help` 是唯一權威。

這條規則有沒有必要？拿 Cloud Run 的 scaling flag 來測：

```bash
$ for f in --min-scale --max-scale --min-instances --max-instances; do
    gcloud help run deploy | rg -q -- "$f" && echo "$f  存在" || echo "$f  不存在"
  done
--min-scale      不存在
--max-scale      不存在
--min-instances  存在
--max-instances  存在
```

`--min-scale` / `--max-scale` 是 Knative 和早期 Cloud Run 的寫法，現在已經不是了。
但這兩個字串在網路上的舊文章、舊 Stack Overflow 回答裡到處都是，
所以模型非常容易吐出一個看起來很合理、跑起來直接報錯的指令。
先跑一次 `gcloud help`，這個問題就消失了。

順帶一提，`gcloud run deploy` 其實沒有 `--dry-run`，只有 `--async`。
skill 的規則是「help 輸出裡有列 `--dry-run` 或 `--validate-only` 就一定要先跑一次」：
有列才跑，沒列就算了。

### 實測二：盲目 list 到底會吃掉多少 context

skill 裡有一條規則我一開始覺得有點囉嗦：

> DO NOT execute any `list` command without including at least one data reduction flag
> (`--limit`, `--filter`, or `--format`).

實際量一下就知道為什麼了。我手上有個 project 跑著 52 個 Cloud Run 服務：

```bash
# A. 盲目 list
$ gcloud run services list --project=YOUR_PROJECT_ID --format=json | wc -c
281918

# B. 只投影需要的兩個欄位（同樣 52 個服務，一個都沒少）
$ gcloud run services list --project=YOUR_PROJECT_ID \
    --format="json(metadata.name,status.url)" | wc -c
7963
```

**281,918 bytes 對 7,963 bytes，差 35 倍。**換算成 token 大約是 70k 對 2k。
也就是說，一個沒加 `--format` 的 `list`，就要吃掉 70k tokens，
而被砍掉的那 97% 是 annotations、conditions、revision template 和一堆時間戳。

skill 還教了一招我覺得很實用的「先探 schema 再查全量」：

```bash
gcloud <GROUP> <RESOURCE> list --limit=1 --format=json
```

先抓一筆看 JSON 長什麼樣，確認欄位路徑之後，再去組 `--filter` 和 `--format`。

其他幾條執行規則也都是同一個思路：

- 一次只跑一個指令，不准 `&&` 串接
- 禁止 pipe、`$( )`、重導向（理由是讓人類 review 指令時看得懂）
- 一律加 `--project=<PROJECT_ID>`，不准依賴 active config 的預設值
- 一律加 `--quiet`，避免在沒有 TTY 的環境卡在互動式確認

### 實測三：哪些指令它不准自己跑

這是我覺得最值得抄的一段。skill 裡有一份明確的 denylist，
列出**沒有人類明確授權就絕對不能自動執行**的操作：

- 任何 IAM policy / role / binding 的異動（權限提升、把自己鎖在門外的風險）
- `gcloud * delete`（不可逆的資源銷毀）
- `gcloud billing *`（帳單爆炸的風險）
- `gcloud organizations *`（組織層級的設定會影響所有人）
- `gcloud kms *`（有機會把資料永久鎖死）
- `gcloud infra-manager deployments apply`（自動跑 IaC 可能砍掉一整批資源）
- 主動啟用 API 也在名單上，理由是開 API 可能連帶開始計費

最後那條我覺得特別有意思。開個 API 聽起來無害，但 skill 直接寫「assume necessary APIs are enabled」，
要開就回頭問人。這種程度的保守，是有認真想過 agent 接上正式環境會發生什麼事的。

### 實測四：Developer Knowledge，查文件而不是憑記憶

plugin 附了一個 MCP server：

```json
{
  "mcpServers": {
    "developer-knowledge": {
      "type": "streamable-http",
      "url": "https://developerknowledge.googleapis.com/mcp"
    }
  }
}
```

這裡遇到第一個坑。MCP server 在設定檔裡宣告了，不代表它真的接得起來。
我這邊列出來的工具只有 `authenticate` 和 `complete_authentication`，
文件講的 `answer_query`、`search_documents` 根本沒出現，代表還卡在認證流程。

有趣的是 skill 自己早就預期到了，白紙黑字寫著：

> **A declared server is not always a connected server.** Some clients cannot complete
> the MCP handshake with this server and expose no `answer_query`, `search_documents`
> or `get_documents` tool at all, even though the plugin declares one.

然後它直接給了 REST fallback。用本來就有的 gcloud 憑證就能打，不用另外申請 API key：

```bash
curl -s -X POST "https://developerknowledge.googleapis.com/v1:answerQuery" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  -H "X-Goog-User-Project: $(gcloud config get-value project)" \
  -H "Content-Type: application/json" \
  -d '{"query": "How do I deploy a container to Cloud Run with gcloud?"}'
```

`HTTP 200`，而且回來的東西比我預期的講究：

```bash
$ jq '.answer | keys' response.json
["answerText", "citations", "references"]

$ jq '.answer.references | length' response.json
10

$ jq -r '.answer.references[].documentReference.documentChunk.document.uri' response.json | sort -u
https://docs.cloud.google.com/run/docs/deploying
https://docs.cloud.google.com/run/docs/quickstarts/deploy-container
https://docs.cloud.google.com/sdk/gcloud/reference/run/deploy
https://docs.cloud.google.com/run/docs/configuring/services/containers
https://docs.cloud.google.com/run/docs/create-jobs
...
```

不只給答案，還附 10 篇官方文件當出處，而且 `citations` 是用字元區間對應到 reference index：

```json
{"startIndex": 447, "endIndex": 505, "sources": [{"referenceIndex": 3}, {"referenceIndex": 7}]}
```

答案裡第 447 到 505 個字是從第 3 和第 7 篇文件來的。這種粒度的引用，要驗證答案真假就容易多了。

查精確語法則用另一個端點，我拿 Cloud Run 的 IAM 權限字串測：

```bash
curl -s -G "https://developerknowledge.googleapis.com/v1/documents:searchDocumentChunks" \
  --data-urlencode "query=cloud run invoker IAM permission run.routes.invoke" \
  --data-urlencode "pageSize=3" \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  -H "X-Goog-User-Project: $(gcloud config get-value project)"
```

撈出來的權限字串：

```
run.routes.invoke
run.services.create
run.services.delete
run.services.get
run.services.list
run.services.update
```

這種 `service.resource.verb` 格式的字串，是模型最容易少一個字或多一個 s 的地方，查一下比猜穩多了。

小坑記一下：skill 自己的 fallback 文件寫回應要從 `documentChunks[].content` 讀，
但實際回來的欄位是 `results[].content`。這個小出入本身就很說明問題。
連官方文件都會跟實際 API 對不上，所以「先驗證再動手」這條規則確實有用。

### 實測五：137 個技能怎麼做到不吃 context

`finding-google-skills` 解決的是另一個問題：Google 的技能目錄很大，但不可能全部預載。

它的做法是只留一頁路由表，真正要用的時候才去撈遠端目錄：

```bash
$ curl -sSL https://raw.githubusercontent.com/google/skills/main/index.json -o index.json
$ jq '.skills | length' index.json
137
$ wc -c index.json
84476
```

137 個技能，84KB。全塞進 context 太浪費，所以 skill 教你先用 `jq` 過濾再讀：

```bash
$ curl -sSL https://raw.githubusercontent.com/google/skills/main/index.json \
  | jq -r '.skills[] | select((.name+" "+.description)|test("cloud run";"i")) | .name'
cloud-run-basics
cloud-run-alert-configuration
cloud-monitoring-metric-selection
google-cloud-global-frontend-configuration
...
```

選出最多三個最相關的，才去抓它們的 entrypoint。這個「目錄放遠端、按需載入」的模式，
我覺得是目前處理大量 skill 最合理的解法。

還有一個細節：這個 skill 明文禁止在憑證驗證失敗時用 `curl -k` 繞過，
連 PowerShell 的 `ServicePointManager` callback 都點名禁止。理由寫得很好：
「你正準備照著抓回來的東西執行指令，一份沒驗證過的目錄比沒有目錄更糟。」

### 五個實際用得上的場景

#### 場景一：把 agent 接上正式環境，但只准它看不准它動

這是我最想要的一個。以前不太敢把 Claude Code 指到 production project，
因為它可能興致一來就幫你「整理」掉一批資源。有了 denylist 之後，界線變得很清楚。

```
幫我看一下 production 專案裡有哪些 Cloud Run 服務超過三個月沒有新的 revision
```

它會去 list、會 filter、會整理成表給你，但碰到 delete 就停下來問。
它省掉的是翻文件和組指令的時間，決定權還在你手上。這個分界抓得很準。

#### 場景二：從零開一個 GCP 專案，把 LINE Bot 丟上 Cloud Run

```
我要開一個新的 GCP 專案部署 LINE Bot，幫我從建專案、綁帳單、開 API 到部署 Cloud Run 走一遍
```

`google-cloud-recipe-onboarding` 會把 billing → project → 啟用 API → 部署整條線帶完。
每個指令動手前先驗語法，開 API 前一定停下來問你（因為在 denylist 上）。
適合久久開一次新專案、每次都要重新 google 一輪的人。

#### 場景三：憑證地獄——本機跑得動，上 Cloud Run 就 403

這大概是 GCP 最常見的鬼打牆。

```
我的服務本機跑正常，部署到 Cloud Run 之後呼叫 Vertex AI 一直 403，幫我釐清是哪個憑證的問題
```

`google-cloud-recipe-auth` 負責釐清 user credential、ADC、service account 三者的關係和適用場合，
Developer Knowledge 負責查到精確的 IAM 權限字串。這兩件事合起來，
比在 Stack Overflow 上翻五年前的答案快多了。

#### 場景四：雲端資源盤點與帳單健檢

```
列出這個專案所有 Cloud Run 服務的名稱、URL 和最後部署時間，找出可能已經沒在用的
```

重點在前面量過的那 35 倍。沒有護欄的話，52 個服務的原始 JSON 就足以讓 agent 在真正開始分析之前，
context 已經被垃圾資料塞滿。有了 `--format` 投影和「先 `--limit=1` 探 schema」的習慣，
52 個服務只吃 2k tokens，資源再多幾倍也還撐得住。

#### 場景五：問新功能時不要再吃到模型的舊記憶

Gemini API、Cloud Run、Firebase 這種迭代很快的產品，模型的訓練資料一定落後。

```
Cloud Run 現在要怎麼設定 min instances 和 startup CPU boost？
```

有了 Developer Knowledge，答案會附上官方文件連結和字元級的引用區間。
更重要的是查不到的時候它會直說：

> Presenting recalled documentation as a retrieved result is the worst available outcome,
> because nothing in the reply distinguishes it from a real lookup.

**「我沒查到，這是憑記憶回答的」比一個聽起來很順但是錯的 flag 有價值太多了。**

### 小結

實測下來，讓我印象最深的其實是兩件跟功能無關的事。

第一，**它整包就是 1549 行 markdown，沒有半行程式碼。**
所有的安全性都靠文字規則實現：先驗語法、限制輸出、明確的 denylist、失敗時誠實說失敗。
這代表你也可以用同樣的方式，把自己團隊的 runbook 和安全規則寫成 skill：
門檻比想像中低很多。

第二，四套 manifest 擠在同一個目錄裡。
Google 沒有為 Antigravity 做專屬格式，而是讓同一包東西在 Claude Code、Codex、Gemini CLI 上都能用。
Skill 正在變成一種不綁定工具的共通格式。

如果你手上有 GCP 專案又在用 coding agent，這個 plugin 裝起來大概五分鐘。
光是不會再亂掰 flag、不會自己去動 IAM 這兩點，就夠划算了。

### 參考連結

- [Set up your Google Cloud developer environment](https://docs.cloud.google.com/docs/get-started/developer-environment?hl=en#antigravity-cli)
- [google/skills - GitHub](https://github.com/google/skills)
- [google-cloud-developer plugin 原始碼](https://github.com/google/skills/tree/main/plugins/cloud/google-cloud-developer)
- [Google Antigravity CLI 文件](https://antigravity.google/docs/cli/)
