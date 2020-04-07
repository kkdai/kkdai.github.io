---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-14 16:21:05+00:00
slug: '%e7%a0%94%e8%a8%8e%e6%9c%83%e5%bf%83%e5%be%97-paas-meetup-2-aws-docker'
title: '[研討會心得] PaaS Meetup #2 AWS Docker'
wordpress_id: 1490
categories:
- 研討會心得
- PaaS
- docker

---

**前言：**




就像上次有提到的，這個meetup主要是由IBM的 BlueMix團隊的人主辦的．  
但是相當有趣的是主要是來研究各種不同的PaaS的部分．這是第二次參加主講的部分是AWS與 Docker


<!--more-->


**心得：**






  * 第一部分：  



    * AWS 的部分很有趣，也開了AWS的帳號．發現建立一個Beantalk 相當的方便．


    * 這次在Coscup裡面也有聽到其實AWS裡面有相當大的一個DevOps的一系列工具．不僅僅能當PaaS連，IaaS都有類似的功能．


    * CLI的操作起來也是學習Heroku，這部分久不需要詳談．這次參加也發現 AWS Beanstalk 其實有支援docker了．



  * 第二部分：



    * 這個部分的講解相當的Cool，主要是利用 Dokku 來架設一個lightweight 的 Heroku在 DigitalOcean上面．


    * 在這裡面docker是為了讓dokku能夠跑出類似heroku的不同dyno 的功能，


    * 類似這樣應用其實挺有趣的，也不斷的思考有沒有什麼特別的應用可以來用．





**AWS (Amazon Web Service)  Beanstalk  ([slide](http://goo.gl/m3PqRX­))**






  * 主講人： 



    * [Moogoo](https://www.facebook.com/moogoo.lee?fref=nf)


    * [QLL](https://play.google.com/store/apps/developer?id=Q.L.L.+Inc.+Ltd.) Engineer using Cornora SDK



  * Amazone Web Service architecture:



    * [http://www.trisys.co.uk/support/documents/AWS/Web%20Application%20Hosting.jpg](http://www.trisys.co.uk/support/documents/AWS/Web%20Application%20Hosting.jpg)



  * 優點：



    * 快速部署，方便管理


    * 整合AWS服務



      * EC2 （運算）


      * S3 （File storage)


      * SNS (message queuing)


      * Cloud Watch 


      * Elastic Load balance


      * Auto Scaling



    * 比較一下:



      * Elastic Beanstalk



        * Application container



      * OpsWorks



        * 利用 shelf 的部署檔，可以把各個主機部署的狀況寫成部署檔來掌控


        * Similar with puppet? [http://puppetlabs.com/](http://puppetlabs.com/)


        * Compare with Puppet and OpsWork[http://www.scriptrock.com/articles/opsworks-vs-puppet](http://www.scriptrock.com/articles/opsworks-vs-puppet)



      * Could Formation



        * Template-driven provisioning




    * Deployment working flow:



      * Create Application -> Upload version -> launch Environment -> manage Environment



    * Deployment command line toolkit (refer [here](http://aws.amazon.com/code/6752709412171743))


    * Demo (Python -> AWS EB with [Flask](http://flask.pocoo.org/))



      * Local run


      * Implement Application.py (for [Flask](http://flask.pocoo.org/))


      * Add Requirement.txt (similar with heroku virtualenv -> pip freeze )


      * init Git



        * Tools : git aws.push



      * eb start



        * Server type 


        * LoadBalance


        * RDS (Relation Database )



      * eb stop (to kill application)



        * **注意： 如果EB 移除的時候，該DB 也會一起被移除．**




        * 也會自動傳一個S3 (作為 log， Version control system)





  * Q&A:



    * Q: 如果主機與本機設定不一樣怎麼處理?


    * A:  



      * 程式內必須處理，PaaS重開後會會把檔案清掉．



    * Q: 要主機執行background command or command line


    * A: 



      * Using EB Workers (like [Heroku worker](https://devcenter.heroku.com/articles/background-jobs-queueing))



    * Q:  各個語言使用的 Web Server 也是不同


    * A:  



      * Python -> Apache, Ruby -> Rails , Java -> Tomcat


      * 所以針對不同web  server 需要跑不同command 也要注意．




  * 公司主要用途：



    * 本來只用AWS EC2，後來都用 AWS EB (對於Startup 有優惠)



  * 參考：



    * [http://www.scriptrock.com/articles/opsworks-vs-puppet](http://www.scriptrock.com/articles/opsworks-vs-puppet)


    * [http://aws.amazon.com/code/6752709412171743](http://aws.amazon.com/code/6752709412171743)


    * EB CLI tool [http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/usingCLI.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/usingCLI.html)





 




**Docker on Dokku on DigitalOcean**






  * 主講人:



    * [Oz Mose ](http://tw.linkedin.com/in/ozmose) System Admin in Faria  Systems



  * Introduce about docker (skip) refer more in [http://docker.com/whatisdocker](http://docker.com/whatisdocker)


  * Docker Ecosystem 



    * Take all operation in Heroku using Docker[Docker](https://github.com/dotcloud/docker) - Container runtime and manager



  * 要架設一個mini-heroku 需要以下的設備



    * [Buildstep](https://github.com/progrium/buildstep) - Buildpack builder


    * [pluginhook](https://github.com/progrium/pluginhook) - Shell based plugins and hooks


    * [sshcommand](https://github.com/progrium/sshcommand) - Fixed commands over SSH


    * nginx


    * git


    * docker



  * It might take 500s



    * Download all image


    * Download docker /lxc-docker


    * Laucn docker on heroku



  * All slide in [https://github.com/mose/20140814-dokku](https://github.com/mose/20140814-dokku)


  * Q&A:



    * Q: 為何使用 docker in dokku 在DigitalOcean? 既然 dokku 已經可以讓自己的電腦可以架起來測試類似PaaS的功能了


    * A:



      * 依舊需要給外部人使用．



    * Q: Dokku 可以有 loadbalance?


    * A:



      * 目前還沒有那麼強大的功能，不過[flynn](https://flynn.io/) 似乎有把比較強大的方式．




