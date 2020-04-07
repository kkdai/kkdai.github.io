---
layout: post
layout: post
author: kkdai
comments: true
date: 2003-09-15 16:11:36+00:00
slug: '%e6%8f%9b%e4%b8%80%e5%80%8b%e6%96%b0%e7%9a%84%ef%bc%8c%e6%b0%b8%e9%81%a0%e6%af%94%e6%9b%b4%e6%96%b0%e4%be%86%e7%9a%84%e7%b0%a1%e5%96%ae'
title: 換一個新的，永遠比更新來的簡單
wordpress_id: 22
categories:
- 學習文件
---

雖然我現在還在讀[研究所](http://www.mis.yzu.edu.tw)但是只要有空，我有在一些公司之中兼差工程師的工讀生

做過很多家公司的工程師
一直覺得工程師不是人搞的
尤其是兼差的工程師
講好聽的是 兼差的工程師
其實根本就是擦屁屁大隊


之前公司裡面的MIS主任（自稱是主任）是從美工起家的
寫的那個程式......

只能有破洞擺出，比初學者更不如的方式來說


最近要幫他們Mail Server 的Apache 與 Php 升級
裡面的過程真是讓我痛苦到受不了～～～
<!-- more -->
現在的情況是這樣
因為我要幫他們裝一套簡單的[WEBMAIL系統](http://nocc.sourceforge.net/)
需要幫他們Mail Server 上面去升級整個Php 升級到有 imap的支援

**直接用tarball 更新**
原本我一開始是直接使用增加IMAP的方式去增加
但是這樣發現了[imap4.7](http://ftp.azc.uam.mx/mirrors/imap/old/imap-4.7.tar.gz)安裝好之後
去安裝PHP4.22 以上的時候
都會出現一些很怪的訊息




  <table cellpadding="0" cellspacing="0" border="1" bgcolor="#000000" width="70%" >
    <tr >
      
<td width="100%" >
        

In file included from /usr/local/php-4.3.3/ext/imap/php_imap.c:46:  

        /usr/local/php-4.3.3/ext/imap/php_imap.h:39: c-client.h: No such file or directory  

        make: *** [ext/imap/php_imap.lo] Error 1


      
</td>
    </tr>
  </table>




即使我換成了  php4.33 或是 更新的版本都會出現這樣的問題


**RPM的安裝**
後來我決定使用RPM來安裝
我到了這個[PHP的RPM網站](http://rpms.arvin.dk/php/rh72/i586/?describe=mod_php#descs)去尋找最新的RPM檔案
於是我開始更新Apache系統
發現在Apache 更新還算是良好
只要是在 Apache1.3.26 的版本上，都還可以
但是一到了PHP4的地方又開始出問題
先是rpm 的版本不夠（需要在rpm 4以上）
當我要更新rpm4的時候，又出現Glibc的版本不足
相互衝突的情況產生（我想大家都一樣吧）


後來我就回頭看了一下
整個系統的需求好像只要有imap就好了
卻不用PHP在4.2以上（似乎最新一些軟體都需要這樣子）


後來我決定重新安裝比較簡單的版本的Apache 與 Php去支援這樣的系統

我在網路上找到一篇比較[詳細的安裝文件](http://www.gouhuo.com/index.php?recid=18&cate=php)


結果我在make Apache 的地方又是出現錯誤




  <table cellpadding="0" cellspacing="0" border="1" bgcolor="#000000" width="70%" >
    <tr >
      
<td width="100%" >
        

make[4]: *** No rule to make target `../../include/alloc.h', needed by `mod_php4.o'.  Stop.  

        make[3]: *** [all] Error 1  

        make[2]: *** [subdirs] Error 1  

        make[2]: Leaving directory `/download/apache_1.3.20/src'  

        make[1]: *** [build-std] Error 2  

        make[1]: Leaving directory `/download/apache_1.3.20'  

        make: *** [build] Error 2


      
</td>
    </tr>
  </table>





現在我惱了
到了現在哪可以錯在這個地方呢？

後來發現裡面竟然沒有這個檔案
所以我決定去用  ln -s 去建立一個強制的鍊結


這樣建立起來的apache 最後終於可以跑動了
更新一個舊的系統 實在很麻煩
尤其是他的lib在安裝的時候都沒有
（或是都是舊的）

要更新真的花了很多的步驟
