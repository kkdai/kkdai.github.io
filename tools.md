---
layout: page
title: 好用小工具
header: Useful Tools
---

這裡收集了我開發的一些實用小工具，旨在解決日常開發或生活中的特定問題。

<style>
  .tool-card {
    border: 1px solid #e1e4e8;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 20px;
    transition: transform 0.2s, box-shadow 0.2s;
    background-color: #fff;
  }
  .tool-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  }
  .tool-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  .tool-title {
    font-size: 1.4em;
    font-weight: bold;
    color: #0366d6;
    margin: 0;
  }
  .tool-links a {
    margin-left: 15px;
    font-size: 0.9em;
    text-decoration: none;
    color: #586069;
  }
  .tool-links a:hover {
    color: #0366d6;
  }
  .tool-description {
    color: #24292e;
    line-height: 1.6;
    margin-bottom: 15px;
  }
  .tool-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }
  .tag {
    background-color: #f1f8ff;
    color: #0366d6;
    padding: 2px 10px;
    border-radius: 12px;
    font-size: 0.8em;
    font-weight: 500;
  }
</style>

<div class="tool-card">
  <div class="tool-header">
    <h3 class="tool-title">🎥 YouTube Channel ID Finder</h3>
    <div class="tool-links">
      <a href="https://youtube-channel-id-finder-660825558664.europe-west1.run.app/" target="_blank"><i class="fa fa-external-link"></i> 線上使用</a>
      <a href="https://github.com/kkdai/youtube-channel-id-finder" target="_blank"><i class="fa fa-github"></i> GitHub</a>
    </div>
  </div>
  <p class="tool-description">
    一個簡潔且具質感的網頁應用，透過 YouTube Data API v3，從任何 YouTube 影片網址中擷取 Channel ID。解決了開發者在串接 API 時難以快速取得頻道識別碼的痛點。
  </p>
  <div class="tool-tags">
    <span class="tag">Next.js</span>
    <span class="tag">TailwindCSS</span>
    <span class="tag">YouTube API</span>
    <span class="tag">Cloud Run</span>
  </div>
</div>

<div class="tool-card">
  <div class="tool-header">
    <h3 class="tool-title">🤖 Agent Skill Hub</h3>
    <div class="tool-links">
      <a href="https://www.evanlin.com/agent-skill-hub/zh/index.html" target="_blank"><i class="fa fa-external-link"></i> 線上使用</a>
      <a href="https://github.com/kkdai/agent-skill-hub" target="_blank"><i class="fa fa-github"></i> GitHub</a>
    </div>
  </div>
  <p class="tool-description">
    Agent Skill Hub 是一個專門收集與展示 AI Agent 技能的平台。這裡彙整了各種實用的技能與工具（如 MCP），讓開發者與使用者能夠更快速地找到並整合所需的 AI 功能，進而強化自動化工作流程與 AI 應用的能力。
  </p>
  <div class="tool-tags">
    <span class="tag">AI Agents</span>
    <span class="tag">MCP</span>
    <span class="tag">Automation</span>
    <span class="tag">Knowledge Base</span>
  </div>
</div>

<div class="tool-card">
  <div class="tool-header">
    <h3 class="tool-title">🎫 數位憑證皮夾 - 訪客背書發證</h3>
    <div class="tool-links">
      <a href="https://did-usecase-visitor-660825558664.asia-east1.run.app/" target="_blank"><i class="fa fa-external-link"></i> 線上使用</a>
      <a href="https://github.com/kkdai/did-usecase-visitor" target="_blank"><i class="fa fa-github"></i> GitHub</a>
      <a href="https://www.evanlin.com/did-visitor/" target="_blank"><i class="fa fa-file-text-o"></i> 說明文章</a>
    </div>
  </div>
  <p class="tool-description">
    一個去中心化身份（DID）的全鏈應用，結合驗證方（Verifier）與發行方（Issuer）的角色。在職員工出示員工卡背書後，系統會當場核發一張具備時效性的臨時訪客通行證至訪客的數位憑證皮夾，安全且兼顧隱私。
  </p>
  <div class="tool-tags">
    <span class="tag">DID</span>
    <span class="tag">數位憑證皮夾</span>
    <span class="tag">Cloud Run</span>
    <span class="tag">Verifier & Issuer</span>
  </div>
</div>

<div class="tool-card">
  <div class="tool-header">
    <h3 class="tool-title">🎬 record2gif</h3>
    <div class="tool-links">
      <a href="https://github.com/kkdai/record2gif" target="_blank"><i class="fa fa-github"></i> GitHub</a>
    </div>
  </div>
  <p class="tool-description">
    一個原生 macOS 螢幕錄影 App，操作方式跟 QuickTime 的「新增螢幕錄影」很像，但錄完可以直接輸出成 GIF。可以錄整個螢幕或框選範圍，錄影時選單列會顯示秒數，按一下或按 ⌃⌘Esc 就停止。停止後會跳出匯出視窗，可以先預覽，再選 GIF 寬度（原尺寸、1280～320 px 或自訂，不會放大）、影格率與是否無限循環。整個 App 用 SwiftUI + ScreenCaptureKit 寫成，沒有第三方套件。
  </p>
  <div class="tool-tags">
    <span class="tag">macOS</span>
    <span class="tag">SwiftUI</span>
    <span class="tag">ScreenCaptureKit</span>
    <span class="tag">GIF</span>
  </div>
</div>

<div class="tool-card">
  <div class="tool-header">
    <h3 class="tool-title">🎵 Song Lingo</h3>
    <div class="tool-links">
      <a href="https://github.com/kkdai/song-lingo" target="_blank"><i class="fa fa-github"></i> GitHub</a>
      <a href="https://www.evanlin.com/gemini-song-lingo/" target="_blank"><i class="fa fa-file-text-o"></i> 說明文章</a>
    </div>
  </div>
  <p class="tool-description">
    用喜歡的歌學語言。貼上 YouTube MV 網址，Gemini 會轉錄歌詞，並逐句加上拼音、繁體中文翻譯、單字拆解與文法說明。每一句都有用 Gemini TTS 設計的老師聲音示範發音，分正常速和慢速。也可以錄下自己跟讀，由 Gemini 3.5 Transcribe 標出哪些詞念對、哪些沒念到。支援日文、韓文與英文。歌詞有版權，所以這是個人學習工具，部署在 Cloud Run 並用 IAP 保護。
  </p>
  <div class="tool-tags">
    <span class="tag">Gemini</span>
    <span class="tag">Gemini TTS</span>
    <span class="tag">Next.js</span>
    <span class="tag">Go</span>
    <span class="tag">Cloud Run</span>
  </div>
</div>

---

*(更多工具持續增加中...)*

