---
layout: post
layout: post
author: kkdai
comments: true
date: 2003-10-06 01:20:23+00:00
slug: '%e5%ae%89%e8%a3%9dwebmail-%e5%ae%8c%e6%95%b4%e8%bb%9f%e9%ab%94-horde'
title: 安裝Webmail 完整軟體 Horde
wordpress_id: 26
categories:
- 學習文件
---

　




日前幫一些客戶看一些軟體，發現他們公司上面的webmail
有一套軟體似乎還不錯用。不僅有『網路信件』、『通訊錄』更有一些行事曆、或是其他的一些功能來使用。而且已經完成中文化（其實現在蠻多opensource的軟體似乎都是這樣的）。




關於這個軟體的簡單介紹，大家可以去該網站觀看一下 
[Horde 的專案網頁](http://www.horde.org/)




　


<!-- more -->


**安裝Webmail 的部分，簡單介紹**





  
  1. 下載Horde 本身的軟體部分：




  
  <table cellpadding="0" cellspacing="0" border="0" width="80%" >
    <tr >
      
<td width="100%" >由於Horde 本身是一個很有系統的軟體，Horde本身只是一個Shell，也就是說
        Horde本身只提供整合那一堆軟體的一個介面。如果要收信的一些功能，還是要下載看是用
        IMP來收信，還是使用Chora的方式來收取信件，本文件主要是利用IMP來收信的部分來做個簡單的介紹。
        

請先到[FTP去下載該軟體的主要部分
        Horde](http://ftp.horde.org/pub/)，不知道的可以下載這個[最新版本的Horde](http://ftp.horde.org/pub/horde/horde-latest.tar.gz)


      
</td>
    </tr>
  </table>
  





　



  
  2. 解壓縮該檔案

  


    
  <table cellpadding="0" cellspacing="0" border="0" width="80%" >
    <tr >
      
<td width="100%" >tar zxvf [horde-latest.tar.gz](http://ftp.horde.org/pub/horde/horde-latest.tar.gz)
        

再來只要簡單的設定他的一些資料就可以
      
</td>
    </tr>
  </table>
    
  




　



  
  3. 設定 Horde本體

  


    
  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="80%" >
    <tr >
      
<td width="100%" >先到 config/ 下
        將檔案名稱有 .dist 的全部去除
        

(horde.php.dist --> horde.php)


        

vi config/horde.php


        

=================================================================


        

// What backend should we use for authenticating users to Horde? Valid  

        // options are currently 'imap', 'ldap', 'mcal', 'sql', 'ftp', 'smb',  

        // 'krb5' and 'radius'.  

        $conf['auth']['driver'] = 'imap'; 
        <==設定你Horde帳戶的登入方式(注意這根webmail
        是不同的)


        


        <==選擇利用IMP登入的話，該MAIL SERVER需要支援IMAP登入'


        


        <==如果選擇 sql 的登入方式，要到 script/將資料庫的
        script 加入到資料庫中  

          

        // An array holding any parameters that the Auth object will need to  

        // function correctly.  

        $conf['auth']['params'] = array();  

          

        // For IMAP, this is the server name, port, protocol, etc.  

        // Protocol is one of 'imap/notls' (or only 'imap' if you have a  

        // c-client version 2000c or older), 'imap/ssl', or imap/ssl/novalidate-cert  

        // (for a self-signed certificate).  Default ports are 143 for imap and  

        // 993 for imap over ssl.  

        $conf['auth']['params']['dsn'] = '{pop3.mailserver.com:143/imap}INBOX';  
        <==設定你用 來檢驗的EMAIL SERVER
      
</td>
    </tr>
  </table>
    
  




　



  
  4. 這樣你的Horde就可以了，但是你會發現當你登入
    http://localhost/horde
    以後，怎麼沒有出現WEBMAI的畫面呢？不要緊張，那是因為你只有安裝好Horde
    的外殼，如果要安裝裡面的服務，還要一個個去安裝。所以回到我們[下載的地方 ](http://ftp.horde.org/pub/)
    　下載 IMP

 

　



  
  5. 下載IMP（也就是Horde 中用來收發webmail
    的部分）可以直接下載[最新版本imp-latest.tar.gz](http://ftp.horde.org/pub/imp/imp-latest.tar.gz)



　



  
  6. 請將這個檔案解壓縮到 horde/imp 之下

  

　




  
  7. 設定好您的 IMP




  
  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="80%" >
    <tr >
      
<td width="100%" >先到 horde/imp/config/ 下
        將檔案名稱有 .dist 的全部去除
        

vi horde/imp/config/servers.php


        

=================================================================


        

// Any entries whose key value ('foo' in $servers['foo']) begin with  

        // '_' (an underscore character) will be treated as prompts, and you  

        // won't be able to log in to them. The only property these entries  

        // need is 'name'. This lets you put labels in the list, like this  

        // example:  

        $servers['_prompt'] = array(  

    'name' => _('pop3') 
        <==要使用那些方式去收發Webmail，剩下的就在下面設定就可以了  

        );


        

$servers['imap'] = array(  

    'name' => 'IMAP Server',  

    'server' => 'example.com',  

    'protocol' => 'imap/notls',  

    'port' => 143,  

    'folders' => 'mail/',  

    'namespace' => '',  

    'maildomain' => 'example.com',  

    'smtphost' => 'example.com',  

    'realm' => 'example.com',  

    'preferred' => ''  

        );  

         
        <==將你的信件設定替換調
        example.com  

        


      
</td>
    </tr>
  </table>
  



  

　


  
  8. 啟動 IMP這個子服務。由於IMP是Horde
    的一個子服務，所以我們需要告知 Horde他已經啟動了。才能正確的啟動這個服務。





  
  <table cellpadding="0" cellspacing="0" border="1" bgcolor="#000000" width="80%" >
    <tr >
      
<td width="100%" >vi config/registry.php
        

=========================================================================


        

$this->applications['imp'] = array(  

    'fileroot' => dirname(__FILE__) . '/../imp',   

    'webroot' => $this->applications['horde']['webroot'] . '/imp', <==確定目錄是正確的  

    'icon' => $this->applications['horde']['webroot'] . '/imp/graphics/imp.gif',  

    'name' => _("Mail"),  

    'allow_guests' => false,  

    'status' =>
        'inactive' 
        <==將 inactive 改成 active  

        );


        

　


        

　


        

　
      
</td>
    </tr>
  </table>
  





　



  
  9. 這樣簡單的設定，就可以跑起來 Horde
    這套具有強大功能的網路辦公室軟體了！！


