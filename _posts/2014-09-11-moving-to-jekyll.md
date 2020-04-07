---
layout: post
layout: post
title: "[Jekyll]從Wordpress搬家到github pages"
description: ""
category:
- ruby
- python
- 學習文件
- Jekyll
tags: []
---

**前言:**

最近買了一台[Synology NAS(DS213J)](https://www.synology.com/zh-tw/products/DS213j)回來用，本來是打算在上面架設部落格跟相簿．不過使用過一下之後，發現[Synology](https://www.synology.com)的限制很多．不僅內建Ｗordpress有許多的問題．更難去設定domain name．加上最近寫部落格覺得使用HTML越來越礙眼．可能跟一直使用[Github](http://github.com)也是有關係的． 所以決定把所有的相簿語部落格全部轉到免費的地方．

-  相簿轉到 [Flickr](https://www.flickr.com/)去，順便做異地備份．不過會把所有之前的資料寫進EXIF．這也是前幾天在忙的事情，應該過兩天會整理一篇文章．
-  部落格轉到[Github Page](https://pages.github.com/)去，其實已經很多大大都是這麼做的，我也很想做很久．
-  最後，家裡的NAS應該會躲在防火牆的後面．專心的儲存檔案．

接下來就會稍微提到，我這幾天在弄[Jekyll](http://jekyllrb.com/) 與 [Hugo](http://hugo.spf13.com/)的心得． 
<!--more-->


**Hugo系統的安裝**

[Hugo](http://hugo.spf13.com/) 的安裝其實沒有遇到太大的問題．唯一的問題就是沒有比較好用的Wordpress導入的工具．大部份都是從[Jekyll](http://jekyllrb.com/)轉過來，所以決定先弄弄[Jekyll](http://jekyllrb.com/) ．

**關於Jekyll的導入：**

- 關於架設Jekyll其實相當簡單，主要就是卡在Ruby架設跟Ruby Gem的安裝
  -  這裡我重新安裝兩次的Ruby跟Ruby Gem才能順利跑到比較正確的版本安裝．
  
- 要啟動Jekyll 主要就是靠 [Jekyll Bootstarp](http://jekyllbootstrap.com/)．
  -  其實Git Clone下來後稍微修改_config.yaml就可以直接使用．

- 關於Jekyll 導入有很多的方向可以走:
  -  使用 Jekyll-import
     -  這邊使用上沒有太大問題，除了文章會變成亂碼的檔案名稱外．再來就是類別沒有輸出． 大失敗．
   
  -  使用Wordpress Jekyll Exporter
     -  這邊是使用Wordpress的插件來使用，不過我的PHP使用的是5.26 (AppServ 2.5.10)，但是這個插件是使用PHP5.3之後的namespace功能．
     
   - 最後找到[ExutWp](https://github.com/thomasf/exitwp)雖然也有檔案名稱亂掉的問題，但是有輸出類別．
     - 但是圖片就不會抓下來，這倒是有點可惜．不過比起圖片來說，類別不見我比較累啊．
     
     
最後總算成功了，也成功的導入到[Github Page] (http://kkdai.github.com)．搞了兩天再裝系統，卻真正只用到Jekyll不到兩個小時的時間．當然扣除掉我的部落格一千多篇文章的轉檔．

最後還需要把Domain Name轉到Github去，可以參考[這篇文章](http://jingyan.baidu.com/article/36d6ed1f5356f31bcf488314.html)．

- 增加一個檔案是CNAME到Github的目錄下，內容是你的網址．(舉例： example.com)
- 上傳到Github後，稍等一下就會轉址．
- 也要去DNS Server設定的地方去改 CNAME來指向你的位址，然後就是等待啦．

打完收工....


**參考文章:**

- 關於Markdown
    - [關於Markdown的工具](http://single9.net/2013/07/markdown-use-mark-to-write-an-article/)
    - [在Sublime Text 上面編輯Markdown，不過我發現中文會有bug](http://plaintext-productivity.net/2-04-how-to-set-up-sublime-text-for-markdown-editing.html)
    - [另外一篇介紹](http://www.jianshu.com/p/378338f10263)
    
- 關於Jekyll導入有關的
   - [Wordpress 去 Octopress(要改)](http://blog.yorkxin.org/posts/2011/11/26/import-from-wpcom-to-octopress/)
   -  [XDite大大的導入文章](http://blog.xdite.net/posts/2011/10/10/how-to-migrate-blog-from-wordpress-to-octopress)
   -  [WordPress to Jekyll Exporter](https://github.com/benbalter/wordpress-to-jekyll-exporter)
   - [Jekyll Importer](http://import.jekyllrb.com/docs/wordpress/)
  
- 關於Jekyll 設定
  - [如何使用Pygment](http://zyzhang.github.io/blog/2012/08/31/highlight-with-Jekyll-and-Pygments/)
  - [也是Pygment這一篇清楚一點，也有比較多例子](http://havee.me/internet/2013-08/support-pygments-in-jekyll.html)
  - [Jekyll bootstap 這個一定要用](http://jekyllbootstrap.com/)
  
