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

---

*(更多工具持續增加中...)*
