---
layout: post
layout: post
title: "[C++][MySQL]修改對應程式碼來避免SQL Injection"
description: ""
category: 
- 學習文件
- vc++程式設計
tags: []

---

![image](http://1.bp.blogspot.com/-F1vpBuwH-Sw/UcOcccS6Z4I/AAAAAAAAAH8/IrmXviGB2EI/s1600/sql_injection.gif)


最近在做伺服器檢查的時候，發現公司內所寫的幾個背景程式經常莫名其妙出問題．於是開始下來檢查一下，就像前幾篇提到的一樣，我有加上HipChat當做是看門狗的程式來監視並且確實的回報資訊給我．

但是，就算有收到回報訊息．找出容易發生問題的地方才是重點．後來有查到當使用者自己從手機上輸入資料傳到伺服器的時候．就會發生MySQL Query Error ，以下面的範例:

<pre class="prettyprint">
 "SELECT * FROM `test_sql` WHERE `account` LIKE '".$account."' AND `password` LIKE '".$password."'"
</pre>

如果這時候，使用這輸入加上有 " ' "的字元，就會產生MySQL Query Error．甚至於會SQL Injeect就是輸入以下的內容:
<pre class="prettyprint">
   "' OR ''=''#"  
</pre>

這時候唯一的方式就是講 “'“ 轉換成 “\'”才不會讓SQL Qeury發生錯誤或是不可預期的狀態．

其實這相關的方式，在PHP裡面相當的簡單就是加上[addslashes](http://php.net/manual/en/function.addslashes.php)函式．但是在用C++直接存取MySQL的時候，可就沒那麼方便．

找了一下相關的文章，後來在MySQL API Document 裡面有找到[mysql_escape_string](http://dev.mysql.com/doc/refman/5.5/en/mysql-escape-string.html)，順便也找到有人寫的範例程式可以使用． 引用自這裡[http://www.3dbuzz.com/forum/threads/146155-C-version-of-addslashes](http://www.3dbuzz.com/forum/threads/146155-C-version-of-addslashes)

<pre class="prettyprint">
/** 
* escapes the mysql string 
* note running on the entire SQL screws it so just run it on elements to be inserted 
* @param string sql 
* @author D.Busby 
*/ 
char *db_connection::escape_string(string sql) 
{ 
        char *s = new char[sql.size()*2 + 1]; 
        mysql_escape_string(s, const_cast<char *>(sql.c_str()), sql.size()); 
        return s; 
}  
</pre>

接下來只要將每個要帶入查詢的內容先加上這個函式就可以．


**相關資源：**

- 講解基本SQL Injection 的範例與原因
    - [http://newaurora.pixnet.net/blog/post/166231341-sql-injection-%E7%AF%84%E4%BE%8B(%E7%99%BB%E5%85%A5%E7%AF%84%E4%BE%8B)](http://newaurora.pixnet.net/blog/post/166231341-sql-injection-%E7%AF%84%E4%BE%8B(%E7%99%BB%E5%85%A5%E7%AF%84%E4%BE%8B)) 
- MySQL其實提供了一個函式，可以幫你把有威脅性的字元，加上"\"的資源
    - [http://www.3dbuzz.com/forum/threads/146155-C-version-of-addslashes](http://www.3dbuzz.com/forum/threads/146155-C-version-of-addslashes) 
- 撰寫一個可以對應可變參數的函式
    - [http://www.dotblogs.com.tw/simplecestlavie/archive/2013/01/02/86637.aspx](http://www.dotblogs.com.tw/simplecestlavie/archive/2013/01/02/86637.aspx)     
- MySQL mysql_escape_string API document
    - [http://dev.mysql.com/doc/refman/5.5/en/mysql-escape-string.html](http://dev.mysql.com/doc/refman/5.5/en/mysql-escape-string.html)     
- 資安電子報，如何預防SQL Injection 攻擊
    - [http://getpocket.com/a/read/738670748](http://getpocket.com/a/read/738670748) 
- 把一段C++ MySQL 加上解決SQL Injection的程式碼
    - [https://gist.github.com/kkdai/31732f96daa9fd6fb0f7](https://gist.github.com/kkdai/31732f96daa9fd6fb0f7) 
