---
layout: post
layout: post
title: "[Docker]增加一些跟Docker有關的筆記"
description: ""
category: 
- docker
tags: []

---

![image](../images/2014/docker-logo.png)


**前言：**

主要是前一篇ELK(Elasticsearch, Logstash and Kibana)文章發表之後，有人在跟我要docker image．雖然馬上就找到了[logstash的docker hub](https://registry.hub.docker.com/u/pblittle/docker-logstash/)．但是還是很認真的玩了一下docker，順便當作複習．

**常用工具**

這裡記錄一下常用的語法：

- 停止並且刪除所有的container

<pre class="prettyprint">
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
</pre>

- 刪除所有的images

<pre class="prettyprint">
docker rmi $(docker images -q)
</pre>

**關於docker的一些發想**

記錄一些這兩天想到關於docker可能有的更多應用．

-  散佈新的web services/applicaiton 
    -  可以隨時把你看過新的web application 或是service利用docker架成一個自成網路然後直接開始給你認識的人．讓他也可以玩玩看，而不用架個老半天，也不擔心雙方OS的問題．
-  Bug reproduce 環境保留
    - 如果某些特殊的環境下，QA可以透過docker建立好具有問題的環境然後交給RD去解決問題．
    
