---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-06-16 05:08:46+00:00
slug: cpan-%e9%8c%af%e8%aa%a4-cant-use-string-cpantarzip-as-a-hash-ref-while-strict-refs-in-use-at-usrlibperl5585cpantarzippm
title: CPAN 錯誤 –Can’t use string ("CPAN::Tarzip") as a HASH ref while "strict refs"
  in use at /usr/lib/perl5/5.8.5/CPAN/Tarzip.pm
wordpress_id: 880
categories:
- 關於MT的學習心得
- VC++程式設計
---

由"[台北小黑豬的部落格](http://funnyd.idv.tw/blog/index.php?blog=1&page=1&paged=2)"GOOLE 的備份過來~  

Can't use string ("CPAN::Tarzip") as a HASH ref while "strict refs" in use at /usr/lib/perl5/5.8.5/CPAN/Tarzip.pm  
在今天試著在redhat as4 安裝amavis 時  
安裝一些amavisd 必要的module   
發現在做某些更新時出現  
Can't use string ("CPAN::Tarzip") as a HASH ref while "strict refs" in use at /usr/lib/perl5/5.8.5/CPAN/Tarzip.pm line 94.  

我試著  
reload index   
reload cpan   
exit 再進入無效  

後來在eflo.net  
提到要  
install mysql-server mysql mysql-devel php-mysql php-devel (for php-eaccelerator)  
vi /etc/php.ini  
press w to search for memory_limit make it to 48m  
then install cpan   
檢視後有安裝上敘提到的套件  
於是我去檢視/etc/php.ini   
default 為8m   
改成48m   
後  
再進行安裝 似乎就沒有問題了 
