---
layout: post
title: "[TIL] Docker Live Restore: Docker 1.12 讓人驚艷的新功能"
description: ""
category: 
- TodayILearn
tags: ["docker", "TIL", "intel"]

---

<iframe src="//www.slideshare.net/slideshow/embed_code/key/cfvedKHsSbOoF" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/Docker/whats-new-in-docker-112-by-nishant-totla-for-docker-sf-meetup-080316" title="What&#x27;s New in Docker 1.12 by Nishant Totla for Docker SF Meetup 08.03.16 " target="_blank">What&#x27;s New in Docker 1.12 by Nishant Totla for Docker SF Meetup 08.03.16 </a> </strong> from <strong><a target="_blank" href="//www.slideshare.net/Docker">Docker, Inc.</a></strong> </div>


Docker 官方在 1.12 正式 Release  之後也放出了新的 Slide 來講解 1.12 新的功能:

Docker Swarm Mode  無疑是最大的亮點，裡面的Routing Mesh可以讓任何節點都可以找到有 deploy 的 service

除此之外 Live Restore: (p.30) 這大概是 DockerCon 2016 之後新的功能，今天看到同事 Demo 後覺得相當有趣． 只要你在啟動 daemon 的時候加上 --live-restore 的參數． 

Docker Daemon 啟動的 Container 就可以把 Docker Daemon 殺掉，但是可以確保自己不會一併消失．

這樣就可以來 Container 來更新 Docker Daemon 