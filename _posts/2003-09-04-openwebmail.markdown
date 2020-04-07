---
layout: post
layout: post
author: kkdai
comments: true
date: 2003-09-04 22:49:04+00:00
slug: '%e7%b8%bd%e7%ae%97%e8%a8%ad%e5%ae%9a%e5%a5%bd-openwebmail%e4%ba%86'
title: 總算設定好 Openwebmail了
wordpress_id: 14
categories:
- 學習文件
---

今天終於把openwebmail 安裝完畢了
參考好　　[網路上張明泰所寫的安裝手冊](http://netlab.kh.edu.tw/board/view_top.asp?messageid=867)

經過了　一段時間的摸索

總算是對　　CGI 與 PERL 安裝與Apache設定有一點點的瞭解
<!-- more -->
現在正在努力拼命安裝　MT 中







根據這個的設定

其實可以很快的安裝起來


但是對於Apache 設定上面
其實有一些地方需要注意到的

1. 對於CGI目錄的設定
　　　如果是CGI 該目錄就不能瀏覽與看檔案（圖形與文件檔)

2.cgi-bin　與　html 分開的概念
　　　基本上只要把該CGI需要的圖片檔放在html目錄就可以執行了

3.在要安裝的CGI 目錄中（建議放在cgi-bin下)
　　要有以下的權限

AllowOverride None
Options None
Order allow,deny
Allow from all


還有要注意

AddHandler cgi-script .cgi .pl



他們建議的那些配件能安裝也是最好啦
