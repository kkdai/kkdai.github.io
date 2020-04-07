---
layout: post
layout: post
author: kkdai
comments: true
date: 2004-03-23 15:07:43+00:00
slug: '%e7%b8%bd%e7%ae%97%e5%b0%87bbs%e7%9a%84%e8%bd%89%e4%bf%a1%e7%a8%8b%e5%bc%8f%e4%bf%ae%e5%be%a9%e5%a5%bd%e4%ba%86'
title: 總算將BBS的轉信程式修復好了
wordpress_id: 64
categories:
- 學習文件
---

![～～別妨礙我轉信唷～～～](http://www.evanlin.com/blog/archives/0323/cagle0kjh0.gif)


大概有一年的時間，由於研究所課業過於忙碌的關係，所以一直沒有去管理[會計四月天](telnet://bbs.acc.scu.edu.tw)這個BBS站台，但是由於最近總算是將論文弄完，所以開始將BBS站台的轉信功能加以修復，修復了幾天，總算將BBS的轉信功能修復完整，也讓我這個失職的站務盡到一點職責，以下就開始介紹轉信系統是如何運作與如何將轉信系統（INND）啟動與加入一些NEWS，我將這些東西寫成了文件～（我是很喜歡將不懂的東西，弄懂後，寫成一份詳細的文件、放在[我的網站](http://www.evanlin.com)之上）




　


<!-- more -->


**轉信系統(innd)**




　




**前提**：




這份文件主要是基於[INNBBSD的基本文件](http://cbs.ntu.edu.tw/threadread.php/board=installbbs&nums=1397)，後的簡單使用方式，教導大家若是在BBS的使用上，應該如何設定這個系統，至於安裝部分，就請看[INNBBSD的基本文件](http://cbs.ntu.edu.tw/threadread.php/board=installbbs&nums=1397)




　




**關於INNBBSD運作：**




關於INNBBSD這個demean
的運作，主要可分為幾個部分讀取news（bbsnnrp)與發送news(bbsslink)兩個大部分，但是會牽扯到三個設定檔





  <table cellpadding="0" cellspacing="0" border="0" width="50%" >
    <tr >
      
<td width="100%" >
        

**bbsname.bbs** 
        設定貴站的  BBS ID (請儘量簡短)  

        **nodelist.bbs** 設定網路各  BBS
         站的  ID, Address  和  fullname  

        **newsfeeds.bbs**   設定網路信件的  newsgroup board nodelist ...
</td>
    </tr>
  </table>





根據以上的說明，所以首先要將bbsname.bbs加以設定好貴站台獨特的名字（不可以有空白、數字），設定完成之後再來就是要建立要相關對應的BBS站台（或是NEWS站台）檔案中的nodelist.bbs，最後要設定好相關的版面與站台對應的檔案newsfeeds.bbs。將這幾個檔案設定好後，INND才會正常的運作。




　




**設定nodelist.bbs**：




關於設定這個檔案的部分，必須要先確定有可以讀取的NEWS主機位置，哪要如何知道哪些NEWS主機是可以被讀取或是可以寫入的SERVER呢？各位可以到[google](http://www.google.com)上去尋找相關的news
server，若要測試該主機是否有讀取或是存取的權限的話可以利用以下的指令才測試，由於每個NEWS主機的登入PORT都是119，所以透過以下指令可以測試是否有登入的權限，也可以查詢相關的NEWS群組





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="50%" >
    <tr >
      
<td width="100%" >
        

# telnet news.server_name
        119 > news_list


        

list


        

quit
</td>
    </tr>
  </table>





利用以上的指令，可以連接一個NES主機，並且將其有的新聞群組加以LIST出來寫成一個檔案之中，您可能會看到類似以下的一個檔案





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="53%" >
    <tr >
      
<td width="100%" >
        

Trying 163.14.2.20...  

        Connected to news.scu.edu.tw.  

        Escape character is '^]'.  

        200 news.scu.edu.tw InterNetNews NNRP server INN 2.3.2 ready (posting ok).  

        215 Newsgroups in form "group high low flags".  

        junk 0000006102 0000006099 y  

        control 0000000576 0000000577 y  

        control.cancel 0005633172 0005616209 y  

        control.newgroup 0000068567 0000068511 y  

        control.rmgroup 0000015582 0000015563 y  

        scu.test 0000000010 0000000011 y
</td>
    </tr>
  </table>





透過這樣的列出，可以幫助快速去瞭解哪些新聞主機有那一些的新聞群組，回歸正題，若要設定nodelist.bbs這個檔案，就是要參照以下的設定方式





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="53%" >
    <tr >
      
<td width="100%" >
        

SCU news.scu.edu.tw 
        POST(119) 清華資訊 News Server  

        EP news.ep.nctu.edu.tw 
        POST(119) 交大電物NewsServer
</td>
    </tr>
  </table>





其中第一個是你取的代號（等等在newsfeed.bbs會用到）關於PORT的119或是7777（自己），都是可以的取好這些對於新聞主機的代號之後，接下來就是對於轉信版面的設定了，也就是要設定newsfeed.bbs這個檔案。




　




**設定nodelist.bbs**：




對於這個檔案，主要是敘述著轉信版面的信件量與相對應的新聞主機對應名稱，裡面的檔案內容可能會是一個以下的檔案內容：





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="53%" >
    <tr >
      
<td width="100%" >
        

#  newsgroups                   board           news server  

        bbs.op.ktv 
        KTV 
        WD  

        bbs.op.antispam 
        AntiSpam WD  

        bbs.op.domain_name_query name_query WD  

        scuec.bbs_talk 
        tran_test SCUEC  

        #-----------------------------  --------------  -----------  

        tw.bbs.sci.e-commerce 
        e-commerce SCU  

        tw.bbs.sci.finance 
        Finance SCU
</td>
    </tr>
  </table>





首先第一個bbs.op.ktv 這個就是新聞群組的名稱，而這個名稱就是透過之前去測試遠端新聞主機所取得的相關結果，接下來第二個欄位就是轉信的版面，而第三個就是所設定的新聞主機的代號，透過這些設定就可以存取到遠方的新聞主機，以上就是設定的三個檔案內容，接下來要介紹與接送新聞有關的檔案，news.active.bbs。




　




**設定news.active.bbs**：




這個檔案主要是記錄，某個新聞主機內所有轉錄信件的號碼（也就是接收、傳送到第幾封信件的意思），所以檔案內容會像是以下的範例檔一樣





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="53%" >
    <tr >
      
<td width="100%" >
        

tw.bbs.comp.lang.php 
        0000000670 0000000658 y  

        tw.bbs.literal.story 
        0000700936 0000700074 y  

        tw.bbs.sci.e-commerce 0000005166 0000005116 y  

        tw.bbs.sci.finance 
        0000338593 0000332805 y  

        tw.bbs.comp.386bsd 0000307755 0000307580 y
</td>
    </tr>
  </table>





第一個數字是文章的總數，第二個數字文章現在接受到的位置（必須比第一個數字還要小才行），起始值可以將第一個數字設定為2，第二個數字設定為1才會動作唷。




　




**啟動innbbsd**：




在這裡要提醒各位，若是要啟動innbbsd這個demean的話，_必須要用完整目錄的啟動，也就是必須是/home/bbs/innd/innbbsd
這樣的啟動方式_，利用這個方式才能正式的去啟動發信（bbslink)與收信(bbsnnrp）這兩隻程式，來輔助。




　




**啟動bbsnnrp**：




對於bbsnnrp的啟動方式，其實算是比較麻煩的一個方式，也就是必須記得新聞主機的位置，其指令格式將是





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="50%" >
    <tr >
      
<td width="100%" >
        

# bbsnnrp 
        news.scu.edu.tw scu.activebbs.bbs
</td>
    </tr>
  </table>





其中，第一個參數就是新聞主機的位置，第二個就是對應到該新聞主機的設定檔，也就是上面所提到的news.active.bbs這個檔案，利用這樣及可以正確的啟動bbsnnrp來取得信件，若有問題可以使用參數 
-c 來取得最新的數字，已跳過有問題的信件




　




**啟動bblink**：




對於bbslink的啟動方式，相較之下就是比較簡單的一個方式，只要指向自己的目錄即可，如下所示





  <table cellpadding="0" cellspacing="0" border="0" bgcolor="#000000" width="50%" >
    <tr >
      
<td width="100%" >
        

# bbslink /home/bbs
</td>
    </tr>
  </table>





　




　




**後記**：




其實這份文件算是比較簡單的部分，但是要寫下來主要是解釋幾個檔案之前的關係，與測試新聞主機的方式，可以作為以後的紀錄，也希望若是真的幫助到一些人能得到各位的迴響。




　




_Evan 于 2004 
03 23 PM11:50_




　




　
