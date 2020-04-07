---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-01-08 05:54:21+00:00
slug: mt%e9%80%9a%e7%9f%a5%e6%9b%b8%e8%a8%ad%e5%ae%9a%e7%9b%b8%e9%97%9c
title: MT通知書設定相關
wordpress_id: 180
categories:
- 關於MT的學習心得
---

![](http://email-for-kids.com/email-man.gif)

由於之前需要建立有IMAP的EMAIL SERVER所以我將電腦中的[SENDMAIL ](http://www.sendmail.org/)換成了[POSTFIX](http://www.postfix.org/)，設定方式可以參考[鳥哥的私房菜裡面的教學文章](http://linux.vbird.org/linux_server/0390postfix.php)。

![[LOGO]](http://www.postfix.org/mysza.gif)

[POSTFIX](http://www.postfix.org/)算是一套很好設定、並且很容易使用、功能又強大的一套EMAIL SERVER。但是~~問題來了，MT裡面的EMAIL 設定，可是預設為[SENDMAIL ](http://www.sendmail.org/)的說~~~

根據[JEDI大大的書中內容](http://mtbook.net/mtbook_install.html#win32_note)，可以去改為SMTP的寄送方式，並且透過CPAN網路上的套件[MAIL::SENDMAIL](http://cpan.uwinnipeg.ca/module/Mail::Sendmail)就可以完成這件事情，在此也跟大家推薦[POSTFIX](http://www.postfix.org/)因為他很好用，並且還有IMAP的功能，就可以建立像[HORDE](http://www.horde.org)、[OPENWEBMAIL](http://openwebmail.org/)這種需要IMAP架構的WEBMAIL軟體，很好用。
