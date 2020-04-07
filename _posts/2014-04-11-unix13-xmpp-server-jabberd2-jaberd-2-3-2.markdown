---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-04-11 05:33:59+00:00
slug: unix-%e9%97%9c%e6%96%bc%e5%9c%a8ubuntu-13-04%e4%b8%8a%e9%9d%a2%e5%ae%89%e8%a3%9dxmpp-server-jabberd2-jaberd-2-3-2
title: '[UNIX] 關於在Ubuntu 13.04上面安裝XMPP server Jabberd2 (Jaberd-2.3.2)'
wordpress_id: 1378
categories:
- unix
- 學習文件
---

太久沒有去操作UNIX的系統來灌東西，這次決定完整灌一台Ubuntu 13.04來跑一些東西  
[Jabber(後來稱為XMPP)](http://zh.wikipedia.org/wiki/XMPP)是一個開放式的即時通訊協定，之前在公司內部則是有在研究延伸出來的另外一個module [Jingle service](http://en.wikipedia.org/wiki/Jingle_(protocol)).




這次回頭來灌系統盡量都使用[Advanced Packaging Tool](http://en.wikipedia.org/wiki/Advanced_Packaging_Tool)[APT] (ex: apt-get/apt-cache)來完成．  
不過Jabberd2 本身是不方便使用[APT](http://en.wikipedia.org/wiki/Advanced_Packaging_Tool)來裝，因為有牽扯一些設定與更改database 與網路安全的設定  
所以在我的流程裡面還使用tarball 自行來設定跟compile




**小抱怨： **  
只能說～很多東西真的一段時間沒摸就會生疏～ 連個permission 也能搞我半天～更別說一些指令...   
加上近幾年都是在mac上面灌一些東西～有些權限的東西也沒在特別注意...  
記錄一些容易出錯的東西..... 







**_Install command in Ubuntu: 13.04 (using jabberd2 as server)_**






  * sudo apt-get update


  * sudo apt-get apache2


    * Check localhost in web browser for validation





  * sudo apt-get mysql-server


    * It will save your mysql root password.


    * using "mysql -uroot -pPASSWORD" to verify it.





  * sudo apt-get install phpmyadmin


    * Web server to reconfiguration choose "apache2"


    * How to verify installation?


      * Check file exist in /usr/share/phpmyadmin


      * make link, in /var/www


      * ln -s /usr/share/phpmyadmin





    * Web browser open http://localhost/phpmyadmin





  * install jabberd2 (refer for more detail)


    * Install wget first:


      * sudo apt-get install wget





    * Install dependency libraries:


      * sudo apt-get install libexpat1-dev


      * sudo apt-get install libidn11-dev


      * sudo apt-get install libudns-dev


      * sudo apt-get install libgsasl7-dev


      * sudo apt-get install libssl-dev


      * sudo apt-get install libmysqld-dev





    * Download package from web side:



      * wget https://github.com/jabberd2/jabberd2/releases/download/jabberd-2.3.2/jabberd-2.3.2.tar.gz



    * unzip files


      * tar -xvf jabberd-2.3.2.tar.gz





    * Enable MySQL, SSL and debug with configure


      * ./configure --enable-mysql --enable-ssl --enable-debug --enable-sqlite --enable-db





    * Make and install


      * sudo make


      * sudo make install










  * Prepare to setting jabberd2


    * configure system path setting.


      * make sure pid/log folder ownership.


        * su jabber


        * sudo mkdir -p /usr/local/var/jabberd/pid


        * sudo mkdir -p /usr/local/var/jabberd/log


        * chown -R jabber /usr/local/var/jabberd


        * Make sure it works well via "touch"





      * Link path


        * sudo ln -s /usr/local/etc/ /etc/jabberd 













  * 


    * Create MySQL DB for jabberd


      * cd [Install_Source_Path]/tools/ (cd ~/jabberd2-3.2/tools


      * mysql -uroot -p


      * . db-setup.mysql


      * exit mysql, goto http://localhost/phpmyadmin/ to verify if exist a DB name jabberd2


      * ln -s /var/lib/mysql/mysql.sock /tmp/mysql.sock





    * Editing sm.xml for storage setting.


      * Change DB driver from sqlite to mysql


        * sudo vi /etc/jabberd/sm.xml


        * search <driver>sqlite</driver>


        * Change <user><pass> in <mysql> for your mysql account.





      * Change <user><pass> in <router> for your service holder user name. (default setting as jabber)


      * Un-comment  <auto create/> to enable auto create user account via client.


      * Change <id> in the first from "sm" to your server name. (ex: Jabber-Server)





    * Editing c2s.xml


      * sudo vi /etc/jabberd/c2s.xml


      * Change <module> to 'mysql'


      * Change <user><pass> in <router> for your service holder user name. (default setting as jabber)





    * Editing rounter.xml


      * sudo vi /etc/jabberd2/rounter.xml


      * Change <user> for your service holder user name. (default setting as jabber)





    * Editing s2s.xml


      * sudo vi /etc/jabberd/s2s.xml


      * Change <user><pass> in <router> for your service holder user name. (default setting as jabber)


      * Change register server name <id register-enable="mu> to your server name (ex: Jabber-Server)










  * Launch jabber service:


    * Switch to jabber and run /usr/local/bin/jabberd2 -D (if you want to debug)


    * Verify if jabber server works well


      * sudo netstat -an | grep -E '5222|5269'


      * Check if both port should be listened.










  * Visit here for more detail.


    * [https://help.ubuntu.com/community/SettingUpJabberServer](https://help.ubuntu.com/community/SettingUpJabberServer)


    * [http://wiki.jabbercn.org/index.php?title=Jabberd2:%E5%AE%89%E8%A3%85%E5%92%8C%E7%AE%A1%E7%90%86%E6%8C%87%E5%8D%97&variant=zh-hk#.E6.95.B0.E6.8D.AE.E5.AD.98.E5.82.A8.E5.8C.85](http://wiki.jabbercn.org/index.php?title=Jabberd2:%E5%AE%89%E8%A3%85%E5%92%8C%E7%AE%A1%E7%90%86%E6%8C%87%E5%8D%97&amp;variant=zh-hk%23.E6.95.B0.E6.8D.AE.E5.AD.98.E5.82.A8.E5.8C.85)


    * [http://wiki.ubuntu.org.cn/index.php?title=UbuntuHelp:SettingUpJabberServer&variant=zh-hant](http://wiki.ubuntu.org.cn/index.php?title=UbuntuHelp:SettingUpJabberServer&amp;variant=zh-hant)


    * [http://devdoodles.wordpress.com/2009/03/19/installing-jabberd2-and-mysql-on-ubuntu-810/](http://devdoodles.wordpress.com/2009/03/19/installing-jabberd2-and-mysql-on-ubuntu-810/)


    * [http://linux.chinaitlab.com/server/42285.html](http://linux.chinaitlab.com/server/42285.html)


    * [http://jaby.heyxu.com/tw/m/0D0300/wghqis5f](http://jaby.heyxu.com/tw/m/0D0300/wghqis5f)







**_  
_**




**_Verify Jabberd2 server with jabber client_**






  * Using [Pidgin](http://www.pidgin.im/) Client


    * Change "Hosts" setting for host name


      * Editing /etc/hosts in Ubuntu


      * Editing Windowssystem32driveretchost in windows


        * add "yourip  Jabber-Server"













  * 


    * Add users as following note:


      * Remember to check option "Create this new account on the server"


      * When you try to add friend (Buddy), remember to add as name@servername





    * For [this](http://tips.webdesign10.com/free-software/how-use-jabber) for other setting





  * Troubleshooting:


    * When user XMPP client add friend and send message . It will occur "XMPP Message Error Message delivery : (Code 404)"


      * Answer: 


        * Make sure friend ID with "username@servername".













  * 


    * Permission for /user/local/var/jabberd2/pid/...


      * Answer:


        * Make sure you run "jabberd" with user name "jabber"


        * Make sure folder access for user "jabber"











