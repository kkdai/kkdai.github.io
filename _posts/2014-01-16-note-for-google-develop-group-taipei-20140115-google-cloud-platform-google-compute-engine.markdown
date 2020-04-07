---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-01-16 12:36:47+00:00
slug: note-for-google-develop-group-taipei-20140115-google-cloud-platform-google-compute-engine
title: Note for Google Develop Group Taipei 2014/01/15- Google Cloud Platform, Google
  Compute Engine
wordpress_id: 1293
categories:
- 研討會心得
---

**Google Cloud Platform**






  * Slide:


  * 


    * [http://www.slideshare.net/littleq0903/introduction-to-google-cloud-platform/1](http://www.slideshare.net/littleq0903/introduction-to-google-cloud-platform/1)





  * What  is google cloud platform


  * 


    * Using google storage 


    * Goole bandwidth 


    * Cloud encapsulation 





  * Cloud Platform:


  * 


    * Cloud SQL


    * 


      * it is MySQL 5.5





    * Cloud Datastore


    * 


      * NoSQL (Non-Relationshop SQL) storage. Much faster than normally SQL.





    * Cloud Storage:


    * 


      * Protection, similar with Google Drive





    * GAE


    * 


      * It much better performance for now.








  * Full Functional Service


  * 


    * feature:


    * 


      * Scalable 





    * Normally


    * 


      * LAMP


      * Apache2 (three apache )with one  DB.


      * Master/Slave DB mapping with Apache2.








  * PaaS (Platform as a Service)


  * GAE (Google App Engine)


  * 


    * Language:


    * 


      * PHP,JAVA, Python


      * GO: (New language), refer [https://developers.google.com/appengine/docs/go/gettingstarted/helloworld](https://developers.google.com/appengine/docs/go/gettingstarted/helloworld)





    * Communication:


    * 


      * Channel:


      * 


        * Using Web socket





      * Mail:


      * 


        * Using specific mail alias to pass some email to GAE to parsing.





      * XMPP:


      * 


        * message like Facebook and google talk.





      * Outbound socket


      * 


        * Only outbound could pass socket to another service from GAE.








    * Process manager:


    * 


      * Cron Job: Similar with Crontab.





    * Computation:


    * 


      * Image API: For image resizing or scaling.


      * Map-reduce: Like OpenCL or multiple threading.





    * Application:


    * 


      * Big Query:


      * 


        * Terabyte data analysis 


        * SQL-like













**Google Compute Engine:**









  * **IaaS (Infrastructure as a Service) — all related could be plug as component to add/remove.**


  * 


    * **Instance— ** You could create** VM** via compute engine for this. **[Speed]**


    * 


      * Similar with MSFT Azure setting but faster and support command line.





    * **Persistent Disc**— PD **[Storage]**


    * 


      * only 1 for R/W. multiple only support read only.


      * Bound by Zone.


      * Security base on AES-128.





    * **Networking**— It about **communication speed.**


    * 


      * Block port SMTP(port 25) and block SMTP on SSL.


      * No matter your instance is in US or Euro it also could 





    * **API**


    * 


      * JSON, OAuth2, RESTful.








  * Why using Google Compute Engine:


  * 


    * Speed


    * Scale


    * 


      * The computing power could be change or scale it up according to your request.


      * 


        * Using an agent which create by GAE and it could monitor current status


        * And run CLI if working flow is archive the boundary. (Add VM or Add PD).








    * Global footprint:


    * 


      * you could easily to expend your business to whole world.s








  * Google has world record about transfer 1TB data and compute it done within 55 sec.


  * Out side is open API outbound, but it could has its own storage, service, internet. All working on API.


  * Every charge is by request:


  * 


    * Every request has its own storage and internet transfer.





  * Zone different management:


  * 


    * It could be separate to different zone to manage your service.






