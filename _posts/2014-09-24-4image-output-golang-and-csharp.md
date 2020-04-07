---
layout: post
layout: post
title: "[Golang][CSharp][PHP]寫出備份4image與把資料寫入EXIF的工具"
description: ""
category: 
- golang
- c#相關學習 
- php
tags: []

---


**前言:**
之前提過買了NAS之後，先把所有的照片全部傳到NAS上面．接下來就要去面對令人不想管的事情．把接近兩百個類別，數萬張的相片名稱與資訊弄出來．

因為我以前的相簿使用的是[4images](http://www.4homepages.de/)，這是一套挺簡單的PHP相簿．

為了要把4image相簿裡面的資料弄出來.. 想弄個Go MySQL結果Oracle 密碼搞半天～下載個MySQL解開還要6XXMB? 靠～決定換MariaDB 反正在Go上面的Driver都一樣....  但是似乎發生了一些難以解決得問題． 所以最後使用C#來完成EXIF寫入的動作． 

不過似乎最後遇到一些在Windows上面的問題，至於為什麼要用Windows是因為我找到了一個可以修改EXIF的小工具，而他有Windows的．

<!--more-->
 

**流程:**

先試著把資料庫弄出來到Mac上面  

- 安裝 MariaDB (參考[這裡](https://mariadb.com/kb/en/mariadb/documentation/getting-started/compiling-mariadb-from-source/building-mariadb-on-mac-os-x-using-homebrew/#installing-mariadb))
    - brew install mariadb
    - mysql_install_db
    - mysqld_safe --datadir='/usr/local/var/mysql'
    - 修改密碼  mysqladmin -u root password ‘PASSWORD’
    - 把資料匯入
        - mysql -uroot -p DB_NAME —force < File_NAME
- 關於Golang 針對MariaDB的部分
    - 參考[這邊](https://mariadb.com/blog/using-go-mariadb)就可以 
        - 這邊有[範例程式碼](https://gist.github.com/kkdai/c368856da99d2aed5ef8)
    - 不過對於我而言，問題出現了:
        - 資料庫裡面都是亂碼(如果透過 phpMyAdmin 抓資料出來的話)，由PHP直接讀是正確的
        - 發現問題可能出在當初phpAdmin的設定的問題，可以參考[這個原因](http://visdacom.com/Website_Design/index.php?load=read&id=28)

到這裡先決定暫停Golang與MySQL的工作，先專注把資料弄出來:

- 最後決定先用PHP把資料匯出，然後把東西放在個別資料夾:
    - 類別名稱與敘述放成檔案 FOLDER_NAME.txt FOLDER_DESC.txt
    - 相片裡面的敘述跟標題放在 pic_NAME.txt pic_DESC.txt
    - 程式放在 [4image PHP Dumptool](https://github.com/kkdai/Album_Dump_Tools/tree/master/php)

接下來就要寫一個工具把在文字檔案裡面的EXIF資訊寫回相片本身的EXIF，由於沒有比較好的Go-EXIF工具．這裡我找到的事[Exiftool](http://www.sno.phy.queensu.ca/~phil/exiftool/)．它是一套Perl寫的工具，本身可以跨平台．而且看起來功能還挺不賴的．

- 所以要使用Golang 去寫一個Windows 下面裡用command line 操控Exiftool來修改相片的EXIF
    - 這裡又遇到一個問題是，不知道為什麼在Windows下面裡用Golang使用command line 執行的parameter都會多一個 "
    - 有在[Stackoverflow上面詢問](http://stackoverflow.com/questions/25736072/windows-command-line-additional-quotation-mark-in-parameter-in-golang)，不過其他人都遇不到這樣的問題？ 
    - 先停住，改成用C#來寫寫看
- 使用C# 來寫就快多了，大概幾個小時就兜出solution並且可以處理幾萬張照片．不過有幾個東西需要記錄．
    - 需要有短暫的sleep不然太快會讓command line tool failed.
- [這個工具也放出來](https://github.com/kkdai/Album_Dump_Tools)，搭配PHP工具一起使用才能正常作用喔..    


**參考:**

- MariaDB (MySQL) installation guide
    - https://mariadb.com/kb/en/mariadb/documentation/getting-started/compiling-mariadb-from-source/building-mariadb-on-mac-os-x-using-homebrew/#installing-mariadb
- How to import SQL to MySQL
    - http://stackoverflow.com/questions/17666249/how-to-import-a-sql-file-using-the-command-line-in-mysql
- How to connect MariaDB via Go
    - https://mariadb.com/blog/using-go-mariadb
- PhpMyAdmin 瀏覽資料庫內容會呈現亂碼的解決方法
    - http://visdacom.com/Website_Design/index.php?load=read&id=28
    - http://forum.serverzoo.com/showthread.php?t=2416
    - http://www.chou-it.com/info/infra/db/mysql_01.html
