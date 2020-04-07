---
layout: post
layout: post
author: kkdai
comments: true
date: 2003-09-04 23:14:45+00:00
slug: mt%e3%80%80%e4%b9%9f%e5%ae%89%e8%a3%9d%e5%a5%bd%e4%ba%86
title: MT　也安裝好了
wordpress_id: 15
categories:
- 關於MT的學習心得
---

當然既然搞懂了Apache 怎麼去設定CGI之後
所以後來安裝 MT 也就沒有那麼困難了

我透過網路上    [阿基的MT攻略](http://blog.brain-c.com/archives/000001.html)
與 [ mtbook](http://mtbook.net/bookindex.html)
很快就把MT 安裝完成

不過還有幾個地方需要注意一下
<!-- more -->
MT需要注意的地方

在  mt.cfg 中
**1.在輸入方塊中出現不了中文？？**
由於各位都是使用中文所以  NoHTMLEntities 1　一定要打開才不會在　文字框框中顯示出中文

**2.更新不了日記？**
Apache 使用者的問題　確定 Apache 啟動者（通常是 apache/apache)的權限要足夠

**3.對於　 CGI 與  HTML 目錄分工的概念：**
建議CGI 放在  /var/www/cgi-bin/ 放的是網頁程式 CGI　　下
而   HTML  放在  /var/www/html/  放的是 給CGI用的圖片、網頁



其實很想把完整安裝過程寫詳細一點

有機會完整寫一次
