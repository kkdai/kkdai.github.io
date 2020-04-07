---
layout: post
title: "[筆記] 關於網路廣告相關用語紀錄"
description: ""
category: 
- 網路產業
tags: [dsp, dmp, ad tech]
---

![](http://cdn.slidesharecdn.com/ss_thumbnails/displaylumascape2014-140203131812-phpapp02-thumbnail-4.jpg?cb=1391433781)

## 前言

主要是整理一下最近在一個學習領域的Line群組討論到關於網路廣告業界的一些名詞．主要的原因是在討論最近很紅的新聞"[延攬台大「機器學習之神」林軒田，Appier 啟動產學合作計畫](http://www.inside.com.tw/2016/02/02/hsuan-tien-lin-join-appier)" 大家就在討論台灣的AD產業近況與大陸成熟的AD發展．

當然這種討論就有一堆名詞跑出來，其實最近應該也不少[文章](http://www.inside.com.tw/2014/05/30/every-local-advertiser-needs-rtb))有提到．我在這裡只是給自己筆記一下，那些名詞到底有什麼差異．


## 產業演化

- 原本由廣告主(Advertiser)跟發布商(publisher)買廣告．就像是某公司看上彎彎的人氣請他在他的人氣部落格上面放廣告一樣．
	- 問題:
		-  由於廣告主與發布商越來越多，不易管理．
		-  廣告主需要自己找發布商太麻煩．
	-  解決方式:
		-  有人出來搜集需求與供給成立AD Network
- "AD Network"來收集所有廣告主的需求與發布商的供給並且透過分類的方式（將發布商分類）來幫廣告主分發廣告．
	- 問題: 
		- 重複付費，無法精準找到瀏覽者．
	- 解決方式:
		- 透過瀏覽者Cookie，開始精確的分類瀏覽者
- "AD Exchange" 來透過Cookie來仔細分析每個網路上的瀏覽者，並且精準的定位出該使用者有興趣的廣告．
- RTB(Real-Time Bidding) 是AD Exchange會提供的功能，透過競標的方式來讓廣告主針對潛在客戶來競標，價高的人可以投放他的廣告．
	- 問題:
		- 大部分廣告主都是透過中間商來跟AD Exchange溝通與購買廣告．無法找到最佳策略(舉例: 廣告商不知道該在Facebook投廣告，或是Google投廣告才能獲得最佳效益）
		- 廣告主可能自己是發布商，透過AD Exchange 可能會買到自己的廣告．
	- 解決方式:
		- 有個人專門來幫助需求端成立平台，廣告主可以透過該平台直接將需求下給各個不同的AD Exchange
- "DSP" (Demand Side Platform)需求方平台幫助廣告主成立一個平台可以將多個AD Exchange整合後給廣告主選擇與使用．並且DSP也提供RTB
- 同時在另外一方面發布商也有相關的聚合服務叫做SSP (Support-Side Platform)．SSP整合了DSP，AD Exchange與AD Network的需求，並且本身也提供RTB．讓發布商可以更容易地在他的媒體上面將廣告賣到正確的使用者身上．

目前幾個已經知道的名詞會形成以下的狀態．

![](http://orbitsoft.com/blog/wp-content/uploads/2013/06/DSP3-1024x340.png)

(Pic: from orbitsoft)

- 這張圖表是另外一件事情就是: 廣告主透過DSP成為了RTB的client，而發布商透過了SSP成為了RTB的Server負責提供資訊給DSP．

- 當DSP越來越多的時候，ATD(Agency Trading Desk) 就是可以讓廣告主去挑選不同的DSP

- 回過頭來不論是ATD還是DSP，廣告主還是得面對去挑選不同的平台與方式來投放他的廣告．但是如何制訂自己的廣告客群是誰？就是得要透過DMP(Data Management Platform)．
	- DMP分析使用者偏好篩選出何者是潛在客戶．(資料來源: SSP)
	- 透過投放的效益報表來顯示最適合的投放平台．(資料來源: DSP)


應該就這些.....	


## 角色舉例:

- Advertiser: 廣告主
	- Ex:  任何一家需要廣告的企業(Visa, Apple, 微軟...)
- Publisher: 發布商
	- Ex: 具有高瀏覽量的網頁或是App(彎彎的網站，Yahoo 新聞，蘋果新聞...)
- AD Network /  AD Exchange
	- 具有廣告發佈與使用者追蹤能力(cookie)的廠商 (Facebook, Yahoo, Google Adsense)
- DSP(Demand Side Platform) 需求端平台
	- 具有多平台的媒體公司，就有會提供DSP．通常像是比較大媒體公司（ex: Yahoo)其實本身也做DMP．
- DMP (Data Management Platform) 能夠做資料分析，當然最重要的就是資料的個數．
	- [TalkingData的Mobile DMP](https://www.talkingdata.com/)透過自己擁有的8億個App使用者搜集來的資訊可以分析，進行推薦進而讓廣告商能夠更精準的投入廣告．
	- 當然Facebook也是如此，透過不同使用者的Facebook ID可以追蹤使用者的喜好幫助他們分析進而找出更精準的廣告投放方式．
	

## 數據轉錄與心得

避免提到太多個人談話，盡量將數據找到網路上有的資訊．

- 根據[Marketing Technology Landscape Supergraphic (2015)](http://chiefmartec.com/2015/01/marketing-technology-landscape-supergraphic-2015/)的報告．2015年的時候有1876個Advertising Vendor在市場中．
- Google在積極回到China的同時[Google Double Click](http://www.businessinsider.com/google-to-launch-doubleclick-audience-center-2015-4), AD Exchange/ GA / Google DMP看來都有機會回到China並且大舉徵才．（不過跟RD無關 :p)
- 負責公司內部管理AD策略與方向部門叫做Data Strategy，這裡有一些[LinkedIn的Director Data Strategy相關履歷](https://www.linkedin.com/title/director-of-data-strategy)．也就是專門負責調配媒體成本到可編程的購買(shift media budget to programmatic buy)也就是透過DMP來買媒體廣告．
- AD Tech中對於技術與資源的要求都相當的高．其中技術不外乎是AI與ML，而資源就是$與媒體關係(不外乎財經關係）．就這幾點，其實台灣的廠商很難做到DMP，頂多就是DSP．
- 大媒體的DSP(ex: Yahoo）本身也會提供DMP的服務．

## 相關文章:

- [Adpartner介紹RTB](https://www.adpartner.me/rtb.html)
	- 蠻推薦這篇的，雖然是公司的宣傳文宣．但是內容淺顯易懂．
- [什麼是RTB 實時競價廣告？他將是未來趨勢！](http://www.inside.com.tw/2014/05/30/every-local-advertiser-needs-rtb)
- [Cookies, DMPs and Is “Big Brother” Watching You?](http://orbitsoft.com/blog/2014/05/cookies-dmps-and-is-big-brother-watching-you/)
- [十大網路忽悠名詞第十名：RTB，過度重視這些事情才是讓你企業失敗的主因！](http://www.inside.com.tw/2015/01/14/do-not-overestimate-these-10-buzzwords-number-10-is-rtb)
	- 這篇文章不錯，用幾個字馬上讓你了解各個名詞的作用．
- [行銷人都要知道的 DSP迷思求解7大守則: 從行銷人員的情境來瞭解網路廣告業的名詞](http://www.clickforce.com.tw/news/view?id=187)
- [RTB China: 不少關於網路廣告的系列文章部落格](http://www.rtbchina.com/)
- [技术解读 DMP：个性化推荐的基础，移动互联网最宝贵的数据资源](http://36kr.com/p/213425.html)


## 相關slide:


<iframe src="//www.slideshare.net/slideshow/embed_code/key/MZMswbQEz1zwTp" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/stanislavmikhailiyk/what-is-real-time-bidding-dsp-ssp-dmp-atd-itd" title="What is Real time bidding (DSP, SSP, DMP, ATD, ITD)" target="_blank">What is Real time bidding (DSP, SSP, DMP, ATD, ITD)</a> </strong> from <strong><a target="_blank" href="//www.slideshare.net/stanislavmikhailiyk">Stanislav Mikhaylyuk</a></strong> </div>
