---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-06-01 07:24:07+00:00
slug: '%e5%8e%9f%e4%be%86mysql40x%e4%b8%8d%e6%98%afunicode'
title: 原來MYSQL4.0X不是unicode
wordpress_id: 404
categories:
- 我的生活
---

[![MySQL](http://dev.mysql.com/common/logos/mysql_100x52-64.gif)](http://www.mysql.com/)

最近由於想架設一個[DKP](http://forums.cjb.net/wowisu-about27.html)網站，才知道自己主機的[MySQL](http://mysql.org) 的版本竟然太舊而無法support unicode的問題（4.1X才有Unicode support, 4.0X都是外面Unicode 而資料庫裡面還是使用iso-8859-1～～）

所以列出一些參考資料

  * [Upgrading from MySQL 4.0 to 4.1](http://dev.mysql.com/doc/refman/4.1/en/upgrading-from-4-0.html)
  * [**_MySQL: Upgrading MySQL 4.0 to 4.1_**](http://peter-zaitsev.livejournal.com/12083.html)（主要是講解upgrade上面可能遇到的問題）

其實充其量～～　就是為了一個功能

　　　　　　**SET NAMES utf8**

乾脆換成FreeBSD算了～～　反正最近ADSL又改成PPPOE的方式了~~~

找到使用[MySQL](http://mysql.org/) 4.0X版本的[EQDKP](http://eqdkp.com/)的版本(1.30)

繁體中文:[簡單架設DKP網站  
](http://home.educities.edu.tw/jgohabbakuk/www/dkpteacher3.htm)簡體中文:[EQDKP 1.3.0 with ItemStats 中文版](http://www.thewow.cn/soft/1090.html)
