---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-07 16:37:46+00:00
slug: '%e7%a0%94%e8%a8%8e%e6%9c%83%e8%b3%87%e8%a8%8apaas-pivotal-%e8%88%87-heroku%e8%a8%8e%e8%ab%96'
title: '[研討會訊息][PaaS] Pivotal 與 Heroku討論'
wordpress_id: 1482
categories:
- 研討會心得
- PaaS
---

Taiwan PaaS Meetup [http://www.meetup.com/%E5%8F%B0%E7%81%A3-PaaS-%E9%9B%B2%E7%AB%AF%E6%8A%80%E8%A1%93%E4%BA%A4%E6%B5%81%E6%9C%83/about/  
](http://www.meetup.com/%E5%8F%B0%E7%81%A3-PaaS-%E9%9B%B2%E7%AB%AF%E6%8A%80%E8%A1%93%E4%BA%A4%E6%B5%81%E6%9C%83/about/)Meetup PaaS HackPad: [https://paas-tw.hackpad.com/Getting-Started-with-Pivotal-Web-Services-PadMnuFp1vd](https://paas-tw.hackpad.com/Getting-Started-with-Pivotal-Web-Services-PadMnuFp1vd)




**總結：**






  * 相較於Pivotal (Cloud Foundry) ，Heroku可能真的比較廉價．


  * 收費方式都不是算流量：



    * Pivotal : 記憶體


    * Heroku: 依照計算能力 Dyno



  * 自動Scale



    * Pivotal似乎沒有自動調節scale 的能力．


    * Heroki似乎沒有看到



  * 與git的搭配:



    * Pivotal並沒有


    * heroku 不僅僅原生就支援git，也可以直接跟github互動．





**心得：**









  * 這次參加的人數不算多，不過倒是很多高手．有看到ihower 還有似乎是他同公司的mose．可見PaaS再用的人真的都不是DevOP的人而是developer．


  * 文創中心算是不錯的場地，交通也算方便．場地費似乎也不貴．其他人要辦活動挺推的．


  * 看起來Pivotal跟 Heroku沒有太多差異，但是 Heroku有免費的方式， Pivotal卻沒有，可能在初期是可以先用 Heroku 正式上線後再考慮要不要挑到 Pivotal．


  * 有很多的討論都可以看出來PaaS跟docker有更多的交錯，只是接下來接近會走向dokku 還是???







 




 




**Pivotal:**






  * 網站：



    * [http://www.pivotal.io](http://www.pivotal.io)



  * 計價方式與免費部分:



    * 計價方式是根據所使用的總記憶體部分．


    * 免費部分: 2GB RAM，10個market place service(十台service)


    * 基本app 單位 warden



      * 有可能換成 docker container  [http://www.activestate.com/blog/2014/04/docker-and-cloud-foundry](http://www.activestate.com/blog/2014/04/docker-and-cloud-foundry)




  * 部署流程(Deploy workflow):



    * 必須要準備一份 manifest.yml，裡面包括：



      * Disk_quota


      * host, name, path


      * Memory size.


      * command (app entry point)



    * cf push APP_NAME


    * 更多參考 [http://docs.run.pivotal.io/devguide/deploy-apps/deploy-app.html](http://docs.run.pivotal.io/devguide/deploy-apps/deploy-app.html)



  * 可能遇到問題：



    * domain name conflict 



      * 不像是Heroku有自動配置名字，所以很容易跟其他人名稱衝突（在push的時候會發生）


      * 需要修改manifest 去解決這個問題．



    * 使用的語言沒有支援的buildpack (Ex: Scala)



      * 可以使用heroku 的buildpack  



        * cf push -b _heroku_build_pack_ 





  * Q&A:



    * Q: 備份的機制?



      * 比較沒有相關的備份機制．甚至跟git 的互動都沒有．


      * Q: 流量的計算？



        * 由於是靠記憶體來收費，所以任何app在deploy之後就馬上會開始收費（就算沒有人用)




    * 參考:



      * Warden 換成 Docker [http://www.activestate.com/blog/2014/04/docker-and-cloud-foundry](http://www.activestate.com/blog/2014/04/docker-and-cloud-foundry)


      * Go-buildpack  [https://github.com/cloudfoundry/go-buildpack](https://github.com/cloudfoundry/go-buildpack)



        * 總共支援: PHP/GO/RUBY/PYTHON/JAVA/node.JS  [http://docs.run.pivotal.io/buildpacks/](http://docs.run.pivotal.io/buildpacks/)



      * Cloud Foundry [https://github.com/cloudfoundry](https://github.com/cloudfoundry)


      * Pivotal - Rewrite Cloud Foundry from Ruby to Go [https://github.com/cloudfoundry/cli/releases](https://github.com/cloudfoundry/cli/releases)


      * Deploy application on Cloud Foundry [http://pivotallabs.com/deploying-jruby-rails-application-cloud-foundry/](http://pivotallabs.com/deploying-jruby-rails-application-cloud-foundry/)


      * 深入 Cloud Foundry [http://qing.blog.sina.com.cn/2294942122/88ca09aa330004r8.html?sudaref=www.google.com.tw](http://qing.blog.sina.com.cn/2294942122/88ca09aa330004r8.html?sudaref=www.google.com.tw)


      * Cloud Foundry 分佈式架構 [http://blog.lamper.cn/cloud-foundry%E5%88%86%E5%B8%83%E5%BC%8F%E6%9E%B6%E6%9E%84%E6%A6%82%E8%BF%B0/](http://blog.lamper.cn/cloud-foundry%E5%88%86%E5%B8%83%E5%BC%8F%E6%9E%B6%E6%9E%84%E6%A6%82%E8%BF%B0/)






 




Heroku:   /her-OH-koo/ 






  * 架構：



    * Slug Compiler -> Slug



      * Slug -> dyno (based on scale)




  * Dyno 特性:



    * isolated


    * self-healing


    * read-only


    * stateless


    * recycle 24hrs



  * Twelve factors App: [http://12factor.net/](http://12factor.net/)



    * Codebase


    * Dependency


    * Config



      * Store config in environment don’t check-in it in your codebase.



    * Backing Services



      * Treat backing service as a attachment service(可抽換）



    * Build, release and run



      * 每個階段應該要是可以切割的



    * Processes



      * stateless, isolated



    * Port binding



      * via port not specific service. 



    * Concurrency


    * Disposability



      * 快速地啟動，優雅的結束



    * Logs



      * Treat logs as stream



    * Dev/Product Parity



      * 必須把 staging，Dev與 Production 要分得開～但是應該要緊接著．



    * Admin Processes



  * Docker V.S. Heroku  



# Docker vs. Heroku


<table style="margin:0 0 1.714285714rem;padding:0;border-width:0 0 1px;border-bottom-style:solid;border-bottom-color:#ededed;font-size:.857142857rem;vertical-align:baseline;border-collapse:collapse;border-spacing:0;color:#757575;line-height:24px;width:624px;font-family:'Open Sans', Helvetica, Arial, sans-serif;" >
<tbody style="margin:0;padding:0;border:0;font-size:12px;vertical-align:baseline;" >
<tr style="margin:0;padding:0;border:0;vertical-align:baseline;" >DOCKERHEROKU</tr>
<tr style="margin:0;padding:0;border:0;vertical-align:baseline;" >

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Dockerfile
</td>

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >BuildPack
</td>
</tr>
<tr style="margin:0;padding:0;border:0;vertical-align:baseline;" >

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Image
</td>

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Slug
</td>
</tr>
<tr style="margin:0;padding:0;border:0;vertical-align:baseline;" >

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Container
</td>

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Dyno
</td>
</tr>
<tr style="margin:0;padding:0;border:0;vertical-align:baseline;" >

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Index
</td>

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >Add-Ons
</td>
</tr>
<tr style="margin:0;padding:0;border:0;vertical-align:baseline;" >

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >CLI
</td>

<td style="margin:0;padding:6px 10px 6px 0;border-width:1px 0 0;border-top-style:solid;border-top-color:#ededed;vertical-align:baseline;" >CLI
</td>
</tr>
</tbody>
</table>





      * [http://tuhrig.de/docker-vs-heroku](http://tuhrig.de/docker-vs-heroku)




  * 可能遇到問題:



    * 寫入的session/temp file 會被清掉．



      * 寫入的暫存資料，會因為PaaS重置而消失．所以必須放在其他的空間．



    * Dyno 超過一個小時沒有運作，會進入休眠 [https://devcenter.heroku.com/articles/dynos](https://devcenter.heroku.com/articles/dynos)



      * 1 Dyno 512 MB 


      * 如果Sleep 之後的第一次讀取會相當的慢．


      * 很多人會寫另外一個App ，不斷地去Ping 主要的App去預防的sleep．




  * Q&A:



    * 經常heroku push 會出現 No cedar-supported detect [http://stackoverflow.com/questions/8361475/heroku-push-rejected-no-cedar-supported-app-detected](http://stackoverflow.com/questions/8361475/heroku-push-rejected-no-cedar-supported-app-detected)



      * A: 可能是heroku不認識你的server，可以考慮用[Heroku build packs](https://devcenter.heroku.com/articles/buildpacks) 來解決類似的問題．




  * 參考：



    * 演講的頭影片 [http://www.slideshare.net/YuChengWang/heroku-git-push-run](http://www.slideshare.net/YuChengWang/heroku-git-push-run)


    * 休眠相關說明 [https://devcenter.heroku.com/articles/dynos](https://devcenter.heroku.com/articles/dynos)


    * Heroku Build Packs [https://devcenter.heroku.com/articles/buildpacks](https://devcenter.heroku.com/articles/buildpacks)



