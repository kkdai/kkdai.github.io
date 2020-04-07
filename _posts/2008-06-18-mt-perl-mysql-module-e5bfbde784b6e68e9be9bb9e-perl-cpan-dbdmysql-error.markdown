---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-06-18 04:32:08+00:00
slug: mt-perl-mysql-module-%e5%bf%bd%e7%84%b6%e6%8e%9b%e9%bb%9e-perl-cpan-dbdmysql-error
title: MT Perl MySQL Module 忽然掛點 — Perl CPAN DBD:mysql error
wordpress_id: 883
categories:
- 關於MT的學習心得
- 學習文件
---

本來跑好好的MT 去讀取MySQL

結果最近忽然出現

Got an error: install_driver(mysql) failed: Can't load  
'/usr/lib/perl5/vendor_perl/5.8.0/i386-linux-thread-multi/auto/DBD/mysql/mysql  
.so'  
for module DBD::mysql: libmysqlclient_r.so.12: cannot open shared object  
file: No such file or directory at  
/usr/lib/perl5/5.8.0/i386-linux-thread-multi/DynaLoader.pm line 229. at (eval  
4) line 3 Compilation failed in require at (eval 4) line 3. Perhaps a  
required shared library or dll isn't installed where expected at  
/var/www/cgi-bin/mt/lib/MT/ObjectDriver/DBI/mysql.pm line 48

  
我去查的RPM 結果

[root@www RPMS]# rpm -qa|grep -i mysql  
php-mysql-4.3.0-2mdk  
MySQL-client-4.0.11a-5mdk  
MySQL-common-4.0.11a-5mdk  
MySQL-bench-4.0.11a-5mdk  
perl-Mysql-1.22_19-6mdk  
libmysql12-4.0.11a-5mdk  
MySQL-4.0.11a-5mdk

  
使用 CPAN 更新的時候出現 error

  
CPAN.pm: Going to build C/CA/CAPTTOFU/DBD-mysql-4.007.tar.gz  
Can't exec "mysql_config": No such file or directory at Makefile.PL line 76.  
Cannot find the file 'mysql_config'! Your execution PATH doesn't seem  


後來去看文章發現以下文章有很像的問題

[http://bytes.com/forum/thread78379.html](http://bytes.com/forum/thread78379.html)

[http://forums.sixapart.com/lofiversion/index.php/t20654.html](http://forums.sixapart.com/lofiversion/index.php/t20654.html)

本來用CPAN 查了很久~~ 但是怎麼查都會出問題

我在查的時候 我發現我CPAN 的 DBD:mysql 忽然不見了  
但是本來都有安裝的

後來安裝 DBD:mysql 的時候  
發現需要 mysql_config  
查詢之下 需要安裝 mysql-devel 的版本  
那是要 MySQL 4.1 以上的版本才有的  
不過 我的 MySQL 是4.0.11a

後來要看到網路上有人講

要找找 libmysqlclient.so.10

開始去找libmysqlclient.so.10

[http://rpmfind.net/linux/rpm2html/search.php?query=libmysqlclient.so.10](http://rpmfind.net/linux/rpm2html/search.php?query=libmysqlclient.so.10)

裝了不少的RPM 仍然不WORK

最後找到

libmysql12 裡面也有libmysqlclient.so.10

所以強制upgrade libmysql12 就可以了

rpm -U libmysql12-4.0.11a-5mdk 即可
