---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-01-20 18:11:11+00:00
slug: feed2js%e4%b8%8d%e8%83%bd%e7%94%a8%e4%ba%86%ef%bc%9f%ef%bc%9f%ef%bc%9f
title: Feed2JS不能用了？？？
wordpress_id: 188
categories:
- 關於MT的學習心得
---

![](http://jade.mcli.dist.maricopa.edu/feed/images/iLoveRSSBlue.jpg)之前經常使用的[Feed2JS](http://jade.mcli.dist.maricopa.edu/feed/index.php?s=about#)來讀取[del.icio.us ](http://del.icio.us/)所存取的網址，不過剛剛發現卻不能正常讀取了（原來該網站更新版本了）

新版本好像不讓人使用它的CODE，只能下載CODE下來自己執行PHP。但是最重要的程式段" [Magpie RSS](http://magpierss.sourceforge.net/)"卻無法正常下載?? 昏了~~ 那該怎麼辦呢?

所以大家趕快去更改為~[Feedsplitter](http://chxo.com/software/feedsplitter/)，用法可以跟以前一樣，只需要更改自己目錄下的_showdelicious.js _將原來練結中的關於_var deliciousrss_ 後有關[Feed2JS](http://jade.mcli.dist.maricopa.edu/feed/index.php?s=about#)的網址改為以下的部份

將原來[Feed2JS](http://jade.mcli.dist.maricopa.edu/feed/index.php?s=about#)的CODE改為以上就可以，不過顯示格式不太一樣(先這樣檔吧~~~)
