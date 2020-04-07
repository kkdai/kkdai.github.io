---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-02 15:37:46+00:00
slug: docker-taipei-meetup2-%e9%97%9c%e6%96%bcdocker-jenkins-%e8%b7%9f-docker-in-golang
title: '[Docker Taipei Meetup#2] 關於Docker & Jenkins  跟 Docker in Golang'
wordpress_id: 1434
categories:
- 研討會心得
- docker

---

其實是很臨時去參加的，所以一發現有空位馬上就參加，對於Docker本身也是一知半解，也希望能透過這些聚會能有更多的了解．  
這裡簡單的記錄一下我的心得，後面是我的速記：




心得:






  * Docker & Jenkins by Casear Chu



    * 這一篇演講相當的特別，讓你與一般Docker 與Jenkins 能想到的不一樣．


    * 主要的架構是根據在[Docker in Docker](https://github.com/jpetazzo/dind) (DinD) 想出來的．也就是在Docker裡面自己再去建立並且讀取其他的docker．


    * 這裡的方法主要如下：



      * 先建立一個ubuntu docker 去安裝Jenkins


      * 然後Jenkins 有兩個工作專案：



        * 一個是下載node.js 系統的image build 成另一個docker


        * 一個是去下載修改的node.js 程式碼，並且定期地在第一個docker上跑測試




    * 整個方法相當特別，測試完之後．可能需要完全移除才會乾淨．不過簡單又不會影響太多部分．



  * Docker in Golang by Jamie Sa [slide](https://speakerdeck.com/jamessa/docker-in-golang)



    * 主講人相當的有趣的，主要是來探討為什麼docker會用到golang 來撰寫


    * 所以一開始會講到當初 docker 創辦人遇到一個什麼樣的狀況，以及簡單的介紹[LXC](https://linuxcontainers.org/) (docker 系統的核心)


    * 並且很有趣的來觀察[github 上面docker project的](https://github.com/dotcloud/docker)狀態，並且分析第一個submit的整個架構與當初兩個創辦人是如何去思考整個系統的架構．


    * 令人相當驚訝的是第一個docker的submit竟然就是撰寫我們看到的web docker console  (是應該花一點時間好好了解人家的架構設計的用心）








[http://www.meetup.com/Docker-Taipei/events/188846162/](http://www.meetup.com/Docker-Taipei/events/188846162/)  



<!--more-->

Docker & Jenkins by _Casear Chu_









  * Except Jenkins (other CIs) 


  * 


    * Travis


    * Door.io (not sure)





  * Docker in Docker


  * 


    * Scenario:


    * 


      * Jenkins in one Docker


      * Use Jenkins to launch other docker for CI testing





    * WebSite:


    * 


      * [https://github.com/jpetazzo/dind](https://github.com/jpetazzo/dind)





    * Detail:


    * 


      * Launch one ubuntu docker and install docker in this ubuntu.








  * Docker Jenkins


  * 


    * Need apt-get jenkins from ubuntu image not jenkins image


    * Because need Docker in Docker in site. 





  * Jenkins using docker  (two processes) 


  * 


    * [https://docs.docker.com/articles/ambassador_pattern_linking/](https://docs.docker.com/articles/ambassador_pattern_linking/)


    * Docker System building process


    * 


      * Check github periodically 


      * download node.js system(private repository) and build docker





    * node.js image process


    * 


      * Git pull node.js code


      * run docker system to load node.js


      * run test


      * Note about report:


      * 


        * docker run -d (wait test report finished)





      * Using [NVM ](http://blog.wu-boy.com/2013/01/node-version-manager/)(node. version manager) to switch variance node.


      * 


        * Note: npm install will take time.








    * Jenkins might need restart when installed plugin


    * 


      * use _docker stop _and _docker start _for this








  * file mapping


  * 


    * [https://registry.hub.docker.com/_/busybox/](https://registry.hub.docker.com/_/busybox/)


    * docker run -v source folder; mapping folder busybox 


    * using docker -[privileged](https://docs.docker.com/reference/run/)







• [Docker in Golang](https://speakerdeck.com/jamessa/docker-in-golang) by _Jamie Sa_







[https://speakerdeck.com/jamessa/docker-in-golang](https://speakerdeck.com/jamessa/docker-in-golang)_  
_









  * About presenter  Founder 


  * 


    * App:Insta-3D [https://itunes.apple.com/us/app/insta3d-instantly-create-your/id883125430?mt=8](https://itunes.apple.com/us/app/insta3d-instantly-create-your/id883125430?mt=8) )


    * Take photo and send image to cloud server to **find mapping** **3D **modeling.





  * Docker system


  * 


    * Base on[ LXC](https://linuxcontainers.org/) 


    * [PID namespace](http://lwn.net/Articles/259217/) is most import improvement in Linux kernel 2.6.4.for LXC which docker is based on this architecture.





  * **Most of the appeal for me is not the feature that GO has, but rather the features that have been intentionally led out — People said about Golang**


  * Why use Golang for Docker by docker founder ([slide](http://www.slideshare.net/jpetazzo/docker-and-go-why-did-we-decide-to-write-docker-in-go))


  * 

    1. Static compilation


    2. Neutral


    3. It has what we need


    4. full development environment


    5. multi-arch build




  * Docker first version is write console mode in web (as sample in docker.io)


  * **利用Github docker 的change list 來了解整個 Docker 的建立的想法**


  * 


    * 第一個submit 把docker console建立起來，並且根據LXC來創立docker的骨幹





  * Docker  的優點


  * 


    * 更小（不需要整個VM的肥大的系統)


    * 更快  







**Not related but reference:**  





  * [http://blog.hsatac.net/2014/06/working-with-docker-and-jenkins/](http://blog.hsatac.net/2014/06/working-with-docker-and-jenkins/)


  * [https://docs.docker.com/userguide/dockerlinks/](https://docs.docker.com/userguide/dockerlinks/)


  * [http://posts.docker.com/](http://posts.docker.com/) (news about docker)


  * [http://segmentfault.com/a/1190000000366923](http://segmentfault.com/a/1190000000366923)






