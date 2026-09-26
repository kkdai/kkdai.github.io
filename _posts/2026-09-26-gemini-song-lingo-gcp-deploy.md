---
layout: post
title: "[AI 實戰] 把 Song Lingo 搬上 Cloud Run：讓一個放著完整歌詞的網站，真的只有我自己看得到"
description: "上一篇用 Gemini 3.8 Flash TTS 做了一個跟著 MV 學日文的 Web App，這篇把它部署到 GCP：Cloud Run 跑 Next.js 加 Python pipeline、Cloud Storage 掛載成資料夾存歌曲與音檔、IAP 鎖到只剩我一個帳號。「只有我看得到」比想像中難講準：我的第一版設計用了簽署網址，等於在 IAP 旁邊開了一扇後門；個人 Gmail 建的專案開了 IAP 之後每個請求都回 502；還有另一個 Claude 視窗在我不知道的時候，已經把這個服務部署過一次了。這篇記錄最後的四層防護、每一層怎麼驗證，以及這些坑。"
category:
- AI
- Claude Code
tags: ["Cloud Run", "IAP", "Cloud Storage", "GCP", "Next.js", "Claude Code"]
---

# 前情提要

[上一篇](https://www.evanlin.com/gemini-song-lingo/)我用 Gemini 3.8 Flash TTS 做了 Song Lingo：貼 YouTube MV 網址，Gemini 轉錄歌詞、加上拼音翻譯文法，再由一位用 voice design 設計出來的老師一句一句念給你聽。

它一直只跑在我自己的電腦上，但我希望拿起手機就能用。所以這篇要做的事很單純：**把它搬上 Cloud Run，讓手機也能用。**

但這個網站有一個特殊的地方：頁面上是完整的歌詞和翻譯。

---

# 需求：「只有我看得到」要多準確

一般的 side project 部署上去，被別人看到頂多有點尷尬。Song Lingo 不一樣，公開有兩個實際的後果：

- **版權**：上一篇花了一整節講，這是個人學習工具，歌詞只存在我自己的電腦。網站一旦公開，性質就從「個人學習」變成「對外提供歌詞」。
- **費用**：加一首歌會呼叫 Gemini Flash，第一次播放示範音會呼叫 TTS。任何人都能用，就等於任何人都能花我的額度。

所以目標不是「有登入功能」，而是**從頭到尾每一層都確認過，只有我的帳號能碰到歌詞和 API**。

我先比較了兩個方案：

| | A. IAP + 程式內驗證 | B. 不開放外部，用 `gcloud run services proxy` |
|---|---|---|
| 能用的裝置 | 任何瀏覽器，包含手機 | 只有登入 `gcloud` 的電腦 |
| 設定難度 | 中等 | 低 |
| 出錯的可能 | 低 | 最低，根本沒有公開入口 |

B 是最安全的，但手機不能用，而手機正是這次部署的理由。所以選 A。

---

# 第一版設計的漏洞：簽署網址

我最初的規劃是這樣：音檔放 GCS，播放時由 API 產生一個短時效的**簽署網址（signed URL）**，把瀏覽器重新導向過去。好處是音檔不經過 Cloud Run，省流量，GCS 本身也支援 Range 請求。

寫到一半重新想「怎樣才能準確地只有我看得到」時，才發現這是一個漏洞：

**簽署網址在有效期限內，任何拿到網址的人都能直接下載，完全不經過 IAP。**

它本質上是一張不記名的通行證。網址出現在瀏覽器歷史、被貼到哪裡、被某個擴充功能記下來，別人就能繞過前面所有的登入檢查。

**原因與解法**：「只有我能存取」是一整條鏈，強度取決於最弱的那一環。最後的做法是**不使用簽署網址**，音檔一律由 Cloud Run 讀出來再送給瀏覽器，每一次播放都要先通過 IAP 和程式內的驗證。一段音檔 250KB 左右，多繞一段路的流量成本可以忽略。

---

# 架構

| 部分 | 做法 |
|---|---|
| 容器 | 一個映像同時裝 Node 22 和 uv/Python，Next.js 直接呼叫原本的 Python 腳本 |
| 歌曲資料與音檔 | 私人的 Cloud Storage bucket，**掛載成 `/data` 資料夾** |
| API key | Secret Manager，以環境變數提供 |
| 存取控制 | IAP + 程式內驗證 IAP 的簽章 |
| 執行個體 | 最多 1 個，最少 0 個；CPU 持續分配 |

幾個決定的理由：

- **把 bucket 掛載成資料夾**，而不是改寫成呼叫 GCS API。程式原本就是讀寫 `output/` 資料夾，掛載之後只要把 `SONG_DATA_DIR` 指到 `/data`，幾乎不用改程式。
- **只跑 1 個執行個體**。避免重複產生音檔、額度用完時暫停呼叫、加入新歌的進度，這些狀態都存在記憶體裡；掛載的 bucket 也沒有跨執行個體的鎖定。個人使用，1 個就夠，而且這樣上一篇那些防止重複計費的機制才會有效。
- **CPU 持續分配**（`--no-cpu-throttling`）。「加入新歌」是在回應送出後繼續在背景跑轉錄和分析的，Cloud Run 預設會在回應送出後限制 CPU，背景工作會卡住。

---

# 四層防護

最後的存取控制是四層，任何一層單獨出錯，其他層還是擋得住：

1. **Cloud Run 權限**：`--no-allow-unauthenticated`，只有 IAP 的服務帳號能呼叫這個服務。
2. **IAP**：只有被授予 `roles/iap.httpsResourceAccessor` 的帳號能通過，也就是只有我。
3. **程式內驗證**：每個請求都驗證 IAP 附上的簽章標頭，再比對 email。
4. **私人 bucket**：強制禁止公開存取，只有這個服務專用的服務帳號能讀寫。

第三層看起來多餘，IAP 已經擋在前面了。但它防的是「IAP 那一層被設定錯」：有人不小心加了 `--allow-unauthenticated`、IAP 被關掉、或是 ingress 設定改了。這種事一旦發生，第三層就是最後一道門。

## 在 Next.js 16 裡驗證 IAP 簽章

Next.js 16 把 `middleware` 改名成 `proxy.ts`，預設跑在 Node.js 執行環境，剛好適合做這件事：

```ts
export async function proxy(request: NextRequest) {
  if (!process.env.K_SERVICE) return NextResponse.next();

  const result = await checkIapAssertion(request.headers.get("x-goog-iap-jwt-assertion"), {
    audience: process.env.IAP_AUDIENCE,
    allowedEmails: process.env.ALLOWED_EMAILS,
  });
  if (!result.ok) {
    console.warn(`[auth] rejected ${request.method} ${request.nextUrl.pathname}: ${result.reason}`);
    return new NextResponse(result.reason, { status: result.status });
  }
  return NextResponse.next();
}
```

兩個設計重點：

- **用 `K_SERVICE` 判斷是不是在 Cloud Run 上**。這個環境變數是 Cloud Run 自動設定的，本機開發時不存在，所以本機完全不受影響，也不需要額外設一個「關閉驗證」的開關（那種開關遲早會被帶上線）。
- **設定漏掉時一律拒絕**。如果 `IAP_AUDIENCE` 或 `ALLOWED_EMAILS` 沒設定，所有請求回傳 500。設定出錯的結果是「網站打不開」，而不是「網站對外開放」。

驗證本身照 [IAP 的文件](https://cloud.google.com/iap/docs/signed-headers-howto)：ES256 簽章、issuer 是 `https://cloud.google.com/iap`、audience 是 `/projects/專案編號/locations/區域/services/服務名稱`，公鑰從 Google 的 JWK 端點抓。

## 上線前怎麼測

沒有真的 IAP 可以打，所以把金鑰來源做成可以替換的參數，在本機用自己產生的 ES256 金鑰測了 12 種情況：

| 情況 | 結果 |
|---|---|
| 允許的帳號（含大小寫不同） | 通過 |
| 其他帳號、沒有 email | 403 |
| 錯誤的 audience、錯誤的 issuer、偽造的簽章、過期、亂填、沒帶標頭 | 401 |
| 沒設定 audience、允許清單是空的 | 500 |

再用正式版建置跑三種模式：

| 模式 | 結果 |
|---|---|
| 本機（沒有 `K_SERVICE`） | 全部 200 |
| Cloud Run 上，但忘了設定 | 全部 500 |
| Cloud Run 上，設定完整，但沒帶標頭或帶偽造的標頭 | 全部 401 |

---

# 踩坑一：另一個 Claude 視窗已經先做了一半

寫程式寫到一半，`git status` 出現兩個我沒建立的檔案：`Dockerfile` 和 `.dockerignore`。

原來是我不小心開了兩個 Claude Code 視窗，兩邊都在討論 song-lingo，另一邊也聊到了部署，已經寫好了 Dockerfile，用的還是跟我不同的做法：**把 GCS 掛載成資料夾**，而我這邊正在寫一整層儲存抽象，把所有讀寫改成呼叫 GCS API。

比較之後，掛載的做法明顯比較好：幾乎不用改程式，而我那一層抽象唯一的優勢（寫入時的版本檢查）在只有一個執行個體、只有一個使用者的情況下用不太到。所以我刪掉了自己寫的那層，改用掛載。

事情還沒完。部署完之後我發現服務已經是**第 2 個 revision**，第 1 個是當天稍早建立的，而且**已經開了 IAP**。那個視窗不只寫了 Dockerfile，還真的部署過一次。

**原因與解法**：兩個 agent 在同一個 repo、同一個 GCP 專案上各做各的，彼此不知道對方的存在。這次沒出事，是因為動手前有先看 `git status`，部署後有去查 revision 清單和服務的設定，而不是假設「我是第一個」。之後我的習慣是：**同一件事只在一個視窗裡做**，開始部署前先查一次雲端上已經有什麼。

---

# 踩坑二：個人 Gmail 的專案，IAP 會對每個請求回 502

部署完、開了 IAP，打開網址，得到的是 502。

一開始以為是程式掛了，但回應標頭有一行很關鍵的線索：

```
HTTP/2 502
x-goog-iap-generated-response: true

Empty Google Account OAuth client ID(s)/secret(s).
```

`x-goog-iap-generated-response: true` 代表**這個錯誤是 IAP 自己回的**，請求根本沒到我的程式。訊息是 IAP 沒有 OAuth client 可以用。

原因是我的專案**不屬於任何組織**，是用個人 Gmail 建的。IAP 預設使用 Google 管理的 OAuth client，而那個 client 只支援組織內的帳號。這種專案一定要自己建一個 OAuth client：

1. **Google Auth Platform**（OAuth 同意畫面）：目標對象是 External。
2. **憑證 → 建立 OAuth 用戶端 ID → 網頁應用程式**，重新導向 URI 填 `https://iap.googleapis.com/v1/oauth/clientIds/用戶端ID:handleRedirect`。
3. 用 `gcloud iap settings set` 把 client ID 和密鑰套用到這個服務。

第 3 步我是在自己的終端機執行，而不是在 Claude Code 的對話裡跑，這樣用戶端密鑰不會出現在任何對話紀錄裡。

## 同意畫面已經公開了怎麼辦

我的專案裡放了很多其他服務，OAuth 同意畫面早就因為別的應用程式設成公開了。我第一個反應是「公開的話，是不是任何人都能登入？要不要切回測試模式？」

答案是**不用改，也不應該改**：

| 層級 | 負責什麼 | 公開後的影響 |
|---|---|---|
| OAuth 同意畫面 | 確認「你是哪一個 Google 帳號」 | 任何帳號都能完成登入這一步 |
| IAP 存取權限 | 確認「這個帳號能不能用這個服務」 | 不受影響 |
| 程式內驗證 | 再確認一次簽章與 email | 不受影響 |

同意畫面只負責「認出你是誰」，真正決定「你能不能進來」的是後面兩層。切回測試模式反而會影響共用同一個同意畫面的其他服務：只有測試使用者能登入，授權大約 7 天就失效。

**原因與解法**：看到 502 先看回應標頭，`x-goog-iap-generated-response` 能直接告訴你問題在 IAP 還是在你的程式。個人 Gmail 的專案要自己建 OAuth client；同意畫面是否公開，不影響誰能使用服務。

---

# 踩坑三：確保 `.env` 和歌詞不會被上傳

`gcloud run deploy --source .` 會把整個資料夾上傳到 Cloud Build。本機的 `.env` 有 API key，`output/` 有所有歌詞，這兩個絕對不能上傳。

`gcloud` 在沒有 `.gcloudignore` 時會改用 `.gitignore`，而這兩個本來就在 `.gitignore` 裡。但「應該會被排除」不夠，所以我明確寫了一份 `.gcloudignore`，再用 gcloud 本身的指令列出實際會上傳的檔案：

```bash
gcloud meta list-files-for-upload .
```

結果是 48 個檔案，`.env`、`output/`、`node_modules`、`.next` 都是 0。

**原因與解法**：會把檔案送出去的指令，先用工具本身列出「實際會送出什麼」，而不是靠自己推論 ignore 規則。

---

# 幾個會讓你誤判的小事

- **zsh 的萬用字元**：用 `gcloud storage ls -r gs://bucket/**` 數物件，結果是 0。zsh 會先把 `**` 當成本機的萬用字元展開，找不到就報錯，查詢根本沒送出。加上引號才正確：116 個物件，跟本機完全一致。
- **`--format` 取不到的欄位不會報錯**：用 `--format='value(iamConfiguration.publicAccessPrevention)'` 查 bucket 設定，輸出是空的。欄位名稱在新版 gcloud 變了，但不會報錯，只會安靜地給你空白。改用 JSON 輸出才看到 `public_access_prevention: enforced`。

這兩個的共通點是：**「0」和「空白」不等於「沒有」或「沒問題」**。驗證安全設定時，拿到空結果要先懷疑查詢本身。

---

# 驗證：每一層都要實際測過

部署完的驗證清單，每一項都實際跑過：

| 檢查 | 結果 |
|---|---|
| 沒登入就打開首頁、API、音檔 | IAP 回 302，導向 Google 登入 |
| 附上偽造的 IAP 簽章標頭 | 一樣 302，IAP 不接受外部帶進來的簽章 |
| 沒登入就用 POST 加入新歌 | IAP 回 401 |
| 匿名存取 bucket 的檔案和列表 | 403 |
| Cloud Run 的呼叫權限 | 只有 IAP 的服務帳號 |
| bucket 的公開權限 | 沒有 `allUsers`、`allAuthenticatedUsers` |
| 我的帳號登入、播放 | 正常，程式內驗證 0 次拒絕 |
| 其他 Google 帳號登入 | 「You don't have access」 |

其中最後兩項只能在瀏覽器測，而「我的帳號能用」其實是最需要確認的：audience 的格式是照文件填的，填錯的話會變成通過了 IAP、卻被自己的程式擋下來。確認的方式是去 Cloud Run 的 log 搜尋 `[auth] rejected`，結果是 0。

log 裡還看到一件讓我很開心的事：我**直接在雲端加了一首新歌**，轉錄、分析、產生示範音、從 bucket 讀出來播放，整條流程在容器裡都跑通了。

---

# 成本與還沒做的事

| 項目 | 估計 |
|---|---|
| Cloud Run | 沒人使用時縮到 0，不收費；CPU 持續分配只在執行個體啟動期間計費 |
| Cloud Storage | 約 27MB，一個月不到 US$0.01 |
| Cloud Build | 每次部署幾分鐘，在免費額度內 |
| Gemini API | 跟本機時一樣，依使用量計費 |

還有兩件事是我自己要在 Console 做的：

- **限制 API key 的用途**，只能呼叫 Generative Language API。
- **設定預算警示**。前面所有防護都失效時，這是讓損失有上限、而且我會知道的最後一道保險。

另外要注意：bucket 和本機的 `output/` 現在是兩份各自獨立的資料，雲端加的歌不會自動同步回來，需要時用 `gcloud storage rsync` 處理。

---

# 幾個我會帶走的東西

**「只有我能存取」是一條鏈，不是一道門。** IAP 設得再好，旁邊一個簽署網址就能繞過去。檢查存取控制時，要把「資料可以從哪些路徑出去」全部列出來，而不是只看入口。

**設定漏掉時，要讓系統關起來，而不是打開。** 程式內驗證在缺少設定時一律拒絕。未來某次部署漏了一個環境變數，結果會是我自己打不開網站、馬上發現，而不是網站默默對外開放好幾個禮拜。

**看錯誤是誰回的。** 同樣是 502，`x-goog-iap-generated-response` 這個標頭直接把問題範圍從「整個服務」縮小到「IAP 的 OAuth 設定」。

**空結果要先懷疑查詢本身。** 物件數量 0、設定欄位空白，都是查詢寫錯，而不是資源真的不存在。驗證安全設定時，這種誤判的代價特別高。

**同一件事只在一個 agent 視窗裡做。** 兩個 Claude Code 視窗各自對同一個專案動手，這次靠著「動手前先看現況」才沒有互相覆蓋。

**密鑰不要經過對話。** OAuth client 的密鑰、API key 都在我自己的終端機處理，AI 只負責告訴我指令，這樣對話紀錄裡永遠不會有密鑰。

程式碼在 [kkdai/song-lingo](https://github.com/kkdai/song-lingo)，README 的部署段落有完整的指令和驗證清單（值都換成了佔位符）。相關文件：[IAP for Cloud Run](https://cloud.google.com/run/docs/securing/identity-aware-proxy-cloud-run)、[驗證 IAP 簽章標頭](https://cloud.google.com/iap/docs/signed-headers-howto)、[IAP 自訂 OAuth 設定](https://cloud.google.com/iap/docs/custom-oauth-configuration)。
