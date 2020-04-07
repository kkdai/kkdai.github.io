---
layout: post
title: "[TIL][Go][System] 閱讀心得: 扛住100亿次请求？我们来试一试"
description: ""
category: 
- TodayILearn  
tags: ["go"]
---

這兩天最紅的 Golang github 大概就是這篇 ["扛住100亿次请求？我们来试一试"](https://github.com/xiaojiaqi/10billionhongbaos/wiki/%E6%89%9B%E4%BD%8F100%E4%BA%BF%E6%AC%A1%E8%AF%B7%E6%B1%82%EF%BC%9F%E6%88%91%E4%BB%AC%E6%9D%A5%E8%AF%95%E4%B8%80%E8%AF%95)

裡面主要就是根據 2015 春晚上使用的微信要紅包與發紅包的系統裡面提到 ["扛住100亿次请求——如何做一个“有把握”的春晚红包系统？" ](http://www.infoq.com/cn/presentations/how-to-build-a-spring-festival-bonus-system#downloadPdf)


其實系統要求很低，有興趣可以玩玩看:

- Server 端:
	- 這篇文章使用相當低的電腦來接受這樣的壓力測試 
	- Dell R2950, 8 Core, 16G RAM.  
- Client 端:
	- 透過 esxi 來分成 17 台電腦來發送需求

代碼部分並沒有太多複雜的部分，主要是觀念上把所有的需求切割成數個 goroutine 來服務． 主要的代碼可以參考 github 或是最基本的 gobyexample 提供的 [non-blocking channel operation](https://gobyexample.com/non-blocking-channel-operations) 

回過頭來，並不是一般人都有機會來試試看 10 billion 的連線機會．不過比較多的問題都會是怎麼我的電腦連幾萬個 (c1000k) 都撐不住了． 可以參考一下作者另外一篇基本文章:  ["c1000K 實踐報告"](https://github.com/xiaojiaqi/C1000kPracticeGuide/blob/master/docs/cn/c1000K.pdf)

裡面有提到基本系統設定:

- 由於連線的時候其實系統本地都會開啟一個檔案來寫，所以需要修改文件開啟上限 
	- /etc/security/limits.conf 
	- /etc/sysctl.conf
- TCP/IP 的網路優化，可以修改
	- net.core.somaxconn
	- net.core.tcp_max_syn_backlog 
    
關於 tcp/ip 在 web connection 這邊有關的可以參考 [nginx 的優化指南](https://www.nginx.com/blog/tuning-nginx/) 

- 有些其他跟 tcp 接收有關的如下: 
	- `tcp_syncookies`
	- `tcp_max_tw_buckets`
	- `tcp_tw_recycle`
	- `timestamps`
	- `tcp_tw_reuse`
	- `tcp_fin_timeout`
	- `tcp_synack_retries`
	- `tcp_keepalive_time`
	- `tcp_keepalive_intvl`
	- `tcp_keepalive_probes`

其實一路上要優化的部分還有很多，找時間慢慢紀錄一下．


## 參考鏈結:

- [http://marcio.io/2015/07/handling-1-million-requests-per-minute-with-golang/](http://marcio.io/2015/07/handling-1-million-requests-per-minute-with-golang/)
- [http://www.cnblogs.com/ghj1976/p/3763866.html](http://www.cnblogs.com/ghj1976/p/3763866.html)
- [http://www.csdn.net/article/2013-05-16/2815317-The-Secret-to-10M-Concurrent-Connections](http://www.csdn.net/article/2013-05-16/2815317-The-Secret-to-10M-Concurrent-Connections)
- [http://c10m.robertgraham.com/p/manifesto.html](http://c10m.robertgraham.com/p/manifesto.html)
- [http://blog.golang.org/qihoo](http://blog.golang.org/qihoo)
- [https://github.com/gopherchina/docs/blob/master/zh-CN/slides/1-5%20Go%20%E8%AF%AD%E8%A8%80%E6%9E%84%E5%BB%BA%E9%AB%98%E5%B9%B6%E5%8F%91%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E5%AE%9E%E8%B7%B5.pdf](https://github.com/gopherchina/docs/blob/master/zh-CN/slides/1-5%20Go%20%E8%AF%AD%E8%A8%80%E6%9E%84%E5%BB%BA%E9%AB%98%E5%B9%B6%E5%8F%91%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E5%AE%9E%E8%B7%B5.pdf)