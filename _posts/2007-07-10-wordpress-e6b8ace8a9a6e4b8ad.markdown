---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-07-10 03:45:51+00:00
slug: wordpress-%e6%b8%ac%e8%a9%a6%e4%b8%ad
title: WordPress 測試中
wordpress_id: 699
categories:
- WordPress學習心得
---

[![](http://tbn0.google.com/images?q=tbn:p1GuKD01xXXyOM:http://aptgetanarchy.org/files/pictures/wordpress.png)](http://aptgetanarchy.org/files/pictures/wordpress.png)

這個Blog 最近正在測試WordPress 2.2.1，目前遇到比較大的問題如下:



	
  1. **UTF8 支援問題**:
就是WordPress 2.2 Default CharSet設定是UTF8，造成MT所export(備份)出來的資料，可能在Import 之後會變成亂碼。主要原因是MySQL尚未支援utf8 (我的版本過舊沒有超過 4.2)
_**解決方式:**
_打開wp-config.php 將以下設定改掉，重新Import.
//define('DB_CHARSET', 'utf8');^M
//define('DB_COLLATE', '');^M

	
  2. **由MT大量Import文章的問題**:
在Import 的時候，會出現 "out of memory"的錯誤狀況，追究起來~~發現跟PHP設定有關。
_**解決方式:**_
打開php.ini，
memory_limit = 8M  ==> 改成 128M，然後Restart Apache

	
  3. **無法大量刪除文章**:
由於(1)的問題，造成我第一次的Import 必須要全部刪除(都是亂碼)。所以~~ 為了這件事情~ 問遍了大小論壇之後，得到的答案是~~~~ "[砍掉重練](http://blog.woixv.com/?p=464)"。


我本來也考慮MT4~~ 無奈他的Widget我實在是看不懂~~ 而且測試尚未結束~~~~
