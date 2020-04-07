---
layout: post
layout: post
author: kkdai
comments: true
date: 2002-11-06 09:49:36+00:00
slug: oracle-8i-%e5%88%a9%e7%94%a8%e5%85%a9%e5%8f%b0%e9%9b%bb%e8%85%a6%e6%9e%b6%e8%a8%adoracle8i%e8%88%87php-apache%e6%a7%8b%e6%88%903tier-client-server%e7%9a%84%e6%9c%8d%e5%8b%99%e7%be%a4%e7%b5%84insta
title: '[Oracle 8i] 利用兩台電腦架設Oracle8i與PHP APACHE構成3tier Client Server的服務群組(Installation
  Php Apache Oracle at Two Computer to do Client and Server Service)'
wordpress_id: 1263
categories:
- 學習文件
---

# **利用兩台電腦架設Oracle8i與PHP APACHE構成3tier Client Server的服務群組
****_(Installation Php Apache Oracle at Two Computer to do Client and Server Service)
2002 11/6 Evan Lin License BY GPL_**




# 設備：


兩台電腦

P4  1 .7GHZ  1      GB RAM 80GBHD  (Oracle 8i Server and Other oracle Service)

1.      P3 800MHZ  256 MB RAM 30GBHD  (Apache 1.3X + PHP 4.22 )


# 軟體：






    1. Oracle 8i 8.17(8.1701ver) for Database Server        [http://otn.oracle.com](http://otn.oracle.com)

    2. Oracle 8i 8.17(8.16ver)     for Database Client         [http://otn.oracle.com](http://otn.oracle.com)

    3. REDHAT Linux 7.3                                                 [http://www.redhat.com](http://www.redhat.com)

    4. Apache 1.37                                                             [http://www.apache.org](http://www.apache.org)

  5. PHP 4.22                                                                  [http://www.php.net](http://www.php.net)




# 起因：


主要是因為要將伺服器與資料庫電腦分離，達到最好的執行效能。一般來說我們會將Server端執行效能提高，達到所謂的「Thin Client」的定義。所以在這邊我們的Server端電腦配備有一定的記憶體、以搭配Oracle在執行大量運算或是預儲程序(Stored Procedure)運作。
所以我們必須建制兩台電腦，並且將Web Server建制到另外一台電腦上面。利用Apache與PHP建制出來的網路服務，對於Oracle有相當多的資源可以利用有兩種函示組Oracle與OCI8者兩種的Function建制出來的功能相當的完整。並且有相關的Function提供給習慣利用MySQL的設計師，讓你可以快速的轉換。


# 架構：


[![C:AppServwww2documentoracle8iBuild_Oracle_PHP_APACHE.filesimage002](http://farm3.staticflickr.com/2853/9246692532_962065e9b0.jpg)](http://www.flickr.com/photos/82596920@N02/9246692532)

Oracle的Client-Server概念中主要如右圖所示，並且該圖表現出Client與Server間必須要有一個共通的溝通管道在這邊扮演著溝通管道的就是Oracle的溝通服務**Net8**。

在此架構中左圖的Client主要指的就是面對客戶的Application Server (PHP)和中間層(Web Server)。並且可以根據客人的需求改變相關的外觀與應用。

而右邊的Server主要就是指的就是DBMS Server在這邊主要就是指的就是Oracle Database Server我們在這裡使用的是Oracle 8i(8.1701)會使用Oracle的原因是因為Oracle本身的功能強大，並且針對於復原與備份的機制相當的強勁。對於我們在資料庫的運用上，會更加的強勁。

會採取以上的架構主要是要將負載程度分散在兩台電腦上面，系統上的負載主要可以分為兩種：

1.      就是網路上的使用者透過網頁伺服器（Web Server）上面來的負載程度，主要就是透過Web Server處理，顯示與傳遞資訊給使用者知道。

2.      經過網頁伺服器傳遞過來的參數，DBMS接收到後開始針對資料庫作處理。這邊的處理可能是相當大的一個負載量。




# What is NET8：


[![C:AppServwww2documentoracle8iBuild_Oracle_PHP_APACHE.filesimage004](http://farm3.staticflickr.com/2864/9246692516_6a1ac99a81.jpg)](http://www.flickr.com/photos/82596920@N02/9246692516)

Client-Server的架構中，透過網際網路的傳輸就是一個固定的工作，但是網際網路上面的傳輸要如何去處理呢。應該透過哪一個PORT？應該利用怎麼樣的通訊協定？應該找尋怎麼樣的主機名稱？這些的設定值中，我們需要一個中間者去作這些參數的設定與工作的處理。在Oracle架構中我們將這樣的工作全部交給了Net8去處理。





在左圖中我們可以看到，我們可可以看到NET8就是處理Client-Server中傳輸問題協調者。在NET8中最底層的就是網路基底（TNS :Transparent Network Substrate）的網路層來提供客戶來交易與傳輸。在這其中的傳輸協定有TCP/IP IPX/SPX DecNet等等的傳輸協定。只要符合在TNS中有定義好。這些協定的可以處的相當好。


## NET8的相關服務


在NET8中主要有以下幾種的 TNS Listener、TNS connection 其中每一次的連線都是一個TNS Connection 而建立 connection 就是透過 TNS Listener啟動的。而NET8中最重要的三個設定參數就是

1.          HOSTNAME   就是伺服器的名稱或是IP為指

2.          TNSNAMES   TNS服務相關協定與PORT

3.          ONAMES                 當你的Oracle唯一群伺服器時，定義每一台名稱規則




## NET8檔案設定部分


NET8主要的檔案設定都在_ $ORACLE_HOME/network/admin_下面中的兩個檔案。

1.          tnsnames.ora            TNS相關協定設定、服務的位置

2.          sqlnet.ora                 傳輸方法、參數定義與其他系統溝通參數設定



上面來說，這兩個檔案似乎決定了對於Server服務的溝通方式，事實上只要你溝通協定設定的好，你可以將許多的溝通路徑全部設定在其中。到時候你再決定一個TNS NAME就可以針對你要溝通的服務、要溝通的主機去呼叫進行傳輸了。

以上兩個檔案你可以直接用文書編輯軟體去編輯、還是用 _netasst  _去執行NET8 ASSISTANT來設定相關的參數。


### TNSNAMES.ORA


這裡提供一些設定方使有關於這個檔案的說明，該檔案的位址在_$ORACLE_HOME/network/admin_


​    
    TNS_名稱 =
    
    sds(DESCRIPTION =
    
        (ADDRESS_LIST =
    
          (ADDRESS = (sdsPROTOCOL = TCP)(HOST =主機名稱 )(PORT = 1521))
    
        )
    
        (CONNECT_DATA =
    
          (SERVICE_NAME = OSID名稱)
    
        )




**TNS 名稱：**  主要是定義 TNS連線的名稱



**主機名稱：**   主要是定義名稱，可以用IP，名稱根據 _/etc/hosts _的紀錄。

**OSID名稱：** 這個就是安裝Oracle 的時候，Oracle的ID名稱。

預設的port 都是 152，在這個例子中式透過 TCP的溝通方式。

** **


### SQLNET.ORA


檔案的位址在  _$ORACLE_HOME/network/admin_


    AMES.DIRECTORY_PATH= (TNSNAMES,ONAMES,HOSTNAME)
    
    NAMES.DEFAULT_DOMAIN=ORA
    
    SQLNET.EXPIRE_TIME=10
    
    USE_DEDICATED_SERVER=OFF
    
    USE_CMAN= FALSE
    
    LOG_DIRECTORY_CLIENT=C:ORACLEORA81NETWORKLOG
    
    LOG_FILE_CLIENT=sqlnet.log
    
    TNSPING.TRACE_LEVEL=OFF
    
    TNSPING.TRACE_DIRECTORY=C:ORACLEORA81NETWORKTRACE
    
    TRACE_FILE_CLIENT=sqlnet.trc
    
    TRACE_DIRECTORY_CLIENT=C:ORACLEORA81NETWORKTRACE
    
    TRACE_LEVEL_CLIENT=OFF
    
    TRACE_UNIQUE_CLIENT=OFF




在這其中 NAMES.DIRECTORY_PATH= (TNSNAMES,ONAMES,HOSTNAME)這行就是設定三個參數的主要函示。告訴NET8傳輸的參數主要以這三個方法來設定。


# Installations


1.          Oracle Server安裝方式、請參照[我的網頁](http://www.evanlin.com/blog/)

2.          **記得在伺服器端也安裝[****完整版本]**、而不要安裝Client 這樣PHP安裝的時候會因為Library 不夠。無法繼續安裝。

3.          利用 [RPM安裝（參考我的網頁）](http://www.evanlin.com/blog/) 或是一般方式安裝PHP、Apache在安裝就可以了

  
    一般安裝方法
    
    1. .Setup Apache Directory
    
    cd /usr/local/apache_VER
    
    ./configure --prefix=/usr/local/apache
    
    
    
    2. Setup Php and Apache
    
    PHP    ./configure --with-apache=../apache_1.3.27 --with-oracle=$ORACLE_HOME --with-oci8=$ORACLE_HOME --enable-track-vars --enable-sigchild
    
    make
    
    make install
    
    Apache ./configure --prefix=/usr/local/apache --activate-module=src/modules/php4/libphp4.a
    
    make
    
    make install
    
    3. 修改 /usr/local/apache/conf/httpd.conf
    
    USER 與 GROUP 改成ORACLE的啟動人與群組（oracle/ oinstall）
    
    4. 啟動 /usr/local/apache/bin/apachectl start





4.          [利用我提供的一些Function](http://www.evanlin.com/blog/) 去更改你的函示

** **


# Reference:


1.      [Oracle 8 Network Administration](http://posejdon.bg.univ.gda.pl/~void/8Net_Admin.pdf)

2.      [Oracle Net8- From the Client-Side](http://www.dbatoolbox.com/WP2002_06/net8_client_side.pdf)

3.      [Net8, Oracle middleware](http://stroga.anonima.free.fr/pdf/net8.pdf)

4.      [NET8 - Oracle Client - URGENT](http://www.orafaq.com/msgboard/sqlnet/messages/950.htm)

5.      [Oracle Net8](http://storacle.princeton.edu:9001/oracle8-doc/network.805/a58230/toc.htm)


