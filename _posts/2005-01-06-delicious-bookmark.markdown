---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-01-06 13:15:36+00:00
slug: delicious-%e6%9b%b8%e7%b1%a4%e5%b0%8f%e5%bc%8a%e7%97%85
title: del.icio.us 書籤小弊病??
wordpress_id: 178
categories:
- 關於MT的學習心得
---

![](http://del.icio.us/delicious.gif) [del.icio.us](http://del.icio.us/)

最近常常有人跟我講，他的LINK不見了，我就覺得奇怪我自己算是很懶的維護我的[del.icio.us](http://del.icio.us/)上的鏈結，那怎麼會不見呢?仔細追下去看，原來[del.icio.us](http://del.icio.us/)所提供的CSS內容，裡面只會有25筆資料。

大家或許會問，那~~~~~~ 25筆以外的資料怎麼辦?好問題，在我不斷尋找解決方式之下，總算找到一個很鳥、但是算是方便的一個解決方法，就是改原來的JAVA SCRIPT。

原來[del.icio.us](http://del.icio.us/)相關JAVA SCRIPT建立方式，請各位去看[ROYBOY的網頁](http://roy.nicetypo.com/nt/roylee.nsf/contentBypermaLink/4919B5000668853F48256E51003053AB)裡面有很詳細的介紹，在此我就提供各位怎麼讀取25筆資料的方式~~~~

**建立標籤(TAG):**

原來[del.icio.us](http://del.icio.us/)幫你建立每個頁面的時候，都會給你的一個選擇性輸入的欄位(TAG)，而他的作用呢~~~主要就是會幫你做分類的工作，比如說 [http://del.icio.us/kkdai/company](http://del.icio.us/kkdai/company) 就是連接到我公司同事的網頁，這樣一來如果大家超過25個以上的資料，就請將他一一的分標籤。

(是不是很鳥的方式??? 但是~~ 其實這樣也有助於你整理龐大的鏈結)

然後，去修改原來JAVA SCRIPT，多增加幾個deliciousrss，並且多做幾次document.write即可，這樣雖然很鳥，但是改一次JAVA SCRIPT 總比改一堆模板好吧??

var rssfloat = Math.floor(Math.random()*99999);
var deliciousrss = 'http://jade.mcli.dist.maricopa.edu/feed/rss2js.php?src=http://del.icio.us/rss/kkdai/company?' + rssfloat + '&chan=yes&num=0&desc=yes&date=no';
var deliciousrss2 = 'http://jade.mcli.dist.maricopa.edu/feed/rss2js.php?src=http://del.icio.us/rss/kkdai/blog?' + rssfloat + '&chan=yes&num=0&desc=yes&date=no';
var deliciousrss3 = 'http://jade.mcli.dist.maricopa.edu/feed/rss2js.php?src=http://del.icio.us/rss/kkdai/code?' + rssfloat + '&chan=yes&num=0&desc=yes&date=no';


function showdelicious(){
document.write('<script language="JavaScript" src="' + deliciousrss + '"></script>')
document.write('<script language="JavaScript" src="' + deliciousrss2 + '"></script>')
document.write('<script language="JavaScript" src="' + deliciousrss3 + '"></script>')
}

如果有更好的方式，請告知~~~~
