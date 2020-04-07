---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-12-19 12:25:28+00:00
slug: buildbot-still-not-fullfill-for-perfornce
title: Buildbot still not fullfill for Perfornce
wordpress_id: 1006
categories:
- 學習文件
---

[![](http://www.axosoft.com/images/partners/Perforce.gif)](http://www.perforce.com)

 

上一篇文章: [http://www.evanlin.com/blog/archives/001100.html](http://www.evanlin.com/blog/archives/001100.html)

 

就像我[前一篇文章](http://www.evanlin.com/blog/archives/001100.html)一樣，我已經把Buildbot裝好了，並且可以跟P4連線(不過僅僅在於periodic[定期])。但是無法正常的透過Scheudler(也就是無法偵測到有最新的CODE更改來trigger build)。

 

I found Buildbot could not using "Scheduler Scheduler" but work well on "Periodic scheduler". As I check in Buildbot site it maybe three known issue as follows:

 

  
  1. [#101 Set "revision" property when using computeSourceRevision](http://buildbot.net/trac/ticket/101)
   
  2. [#127 got_revision not tracked for perforce](http://buildbot.net/trac/ticket/127)
   
  3. [#229 Build properties "revision" and "got_revision" not populated for P4](http://buildbot.net/trac/ticket/229)
 

Although I already change this by the patch but it sill could not fix the "scheduler". I think I will check with Buildbot team as well.
