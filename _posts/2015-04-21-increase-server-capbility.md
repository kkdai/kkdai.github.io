---
layout: post
layout: post
title: "[Ubuntu][Golang]關於Ubuntu Server連線能力設定"
description: ""
category: 
- golang
- linux
tags: []

---

![image](http://cdn4.cybernetnews.com/wp-content/uploads/2007/08/speedlimit.jpg)

## 前言:

架設Golang Server選擇可以有兩個方向，大部分的人可能選擇自己架設主機來跑Golang Server如果要能夠達到10k甚至是100k連線數的話．這邊把一些經驗記錄下來．

### 架設環境

這邊的伺服器主要是架設在以下的設備與軟體:
- Ubuntu 14.04 X64
- MySQL + Apache + Ejabberd


### 了解主機原先設定的上限與調整

先就不討論硬體架構的連線能力狀況下，還是有一些軟體設定需要調整．

#### MySQL 連線上限調整

首先會遇到的問題就會是MySQL query 卡住的問題，每一次的Query Request 都會卡在MySQL那一端．主要是以下的設定需要調高．`max_connections`原先設定只有`100`．設定方式如下:(細節參考[這裡](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_max_connections))

        mysql> set GLOBAL max_connections=65536;

可能還是會遇到有一些問題，建議把另外一個`max_connect_errors`一併也調高

        mysql> set GLOBAL max_connect_errors=65536;


##### 改完MySQL  接下來可能出現的會是 Ubuntu 設定

改完了MySQL的設定之後，整個設定就可以變得相當的順暢．但是偶爾會出現一些問題，就得接下去追查原因．

#### 問題： 一次能進入的TCP connection 太少

首先可能去做調整就是去`/etc/sysctl.conf`來增加TCP連線數，以下是參考設定．

        net.ipv4.tcp_syncookies = 1
        net.ipv4.tcp_tw_reuse = 1
        net.ipv4.tcp_tw_recycle = 1
        net.ipv4.tcp_fin_timeout = 30
        net.ipv4.tcp_keepalive_time = 1200
        net.ipv4.ip_local_port_range = 1024 65000
        net.ipv4.tcp_max_syn_backlog = 8192


##### 問題: "too many open files"

解決了大量連線數無法接收的問題，卻發生了`"too many open files"`的error．

根據這個[Stackoverlow](http://stackoverflow.com/questions/27847730/dial-error-too-many-open-files)，由於每一個TCP的Connection在Linux系統本身都被當作是一個file來存取，所以面對到大量connection的時候，可能遇到的問題就是依照前面的方式把TCP connection 打開到65000之後，但是由於每一個TCP connection 會對應到一個檔案的開啟，接下來就是檔案開啟的上限被打爆．於是就會出現....

        Too many open files
        
這個問題可能是關於到Ubuntu原本的設定．
    
        ulimit -n //console mode 可以顯示目前可以同時開啟起檔案的最大量． 預設: 1000
        
修改方式主要是打開 `/etc/security/limits.conf`

        *         hard    nofile      500000
        *         soft    nofile      500000
        root      hard    nofile      500000
        root      soft    nofile      500000
               
        
這樣應該可以初步的把server request產生到同時間至少有3000以上．        

### 參考文章

- [MySQL Parameter Setting](http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html)
- [Stackoverlow: dial error : 'too many open files'](http://stackoverflow.com/questions/27847730/dial-error-too-many-open-files)
- [Linux Increase The Maximum Number Of Open Files / File Descriptors (FD)](http://www.cyberciti.biz/faq/linux-increase-the-maximum-number-of-open-files/)
- [Increase “Open Files Limit”](https://rtcamp.com/tutorials/linux/increase-open-files-limit/)
