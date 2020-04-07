---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-07-11 01:24:45+00:00
slug: golang-%e5%bd%b1%e7%89%87%e5%bf%83%e5%be%97%e6%95%b4%e7%90%86-google-io-2012-go-in-production
title: '[Golang] 影片心得整理- Google IO 2012- Go in Production'
wordpress_id: 1450
categories:
- Golang
- Python
- Ruby
- Ruby on Rails
---

[https://www.youtube.com/watch?v=kKQLhGZVN4A](https://www.youtube.com/watch?v=kKQLhGZVN4A)




每次剛學一個新的程式語言，自己的心裡總是會想著“這個新的語言特點是什麼？他究竟能夠為了產品或是你工作的事情上帶來哪些幫助？”  
學習Golang的時候，我也是會這樣想．剛好找到了這一段影片，把它的一些內容做個整理，如下：






  * **[Juju](http://en.wikipedia.org/wiki/Juju_(software)) - Ubuntu (Canonical)**



    * **語言轉換:**



      * Python -> [Golang](http://zh.wikipedia.org/wiki/Go)



    * **這是什麼？**



      * [Juju](http://en.wikipedia.org/wiki/Juju_(software)) 是ubuntu上面管理軟體的一個工具，這裡有[詳細的解釋](http://www.arthurtoday.com/2011/12/ubuntu-juju.html#.U7tDVY2Sz0Q)．



    * **Golang帶給[Juju](http://en.wikipedia.org/wiki/Juju_(software))的好處：**



      * [Juju](http://en.wikipedia.org/wiki/Juju_(software))原本是使用Python來寫的，會換到Golang上面其實有相當多的理由，稍微整理一下:



        * Error Handle: 這大概是基本上Golang最強大的功能． 


        * Concurrency: 這也是Golang被人喜愛的主要原因，目前我也還在努力地摸索．


        * 他其實有更多沒有提到的，我稍微截個圖讓大家看一下  
![JPG juju more](http://www.evanlin.com/blog/wp-content/uploads/2014/07/JPG-juju-more.jpg)





  * [**Heroku**](https://www.heroku.com/)



    * **語言轉換:**



      * Ruby ->  [Golang](http://zh.wikipedia.org/wiki/Go)



    * **這是什麼?**



      * [Heroku](https://www.heroku.com/) 算是現在幾個知名的PaSS (Platform as a Service)之一，主要就是讓人host你的網路相關的service．


      * [這裡有更詳細](http://www.inside.com.tw/2010/09/20/heroku)的解釋．



    * **Golang帶給[Heroku](https://www.heroku.com/)的好處:**



      * [Heroku](https://www.heroku.com/) 主要有一個Routing Table Service  來記錄超過 190萬個App的操作．這樣的service主要就是透過REST API然後與後方的PostgreSQL資料庫．原本是使用Ruby來撰寫一個部分的控制．詳細的流程圖我截取下來如下：  
![JPG Heroku Ruby](http://www.evanlin.com/blog/wp-content/uploads/2014/07/JPG-Heroku-Ruby.jpg)


      * 問題來了，bootstrapping new instance由於要產生一些資料所以是不夠快的，根據講者的說法，這跟Ruby產生initial JSON dump有關．


      * 所以他們使用Golang在幾個小時內完成Streaming update部分的原型並且測試上線， 結果帶來以下的結果:



        * 速度增加十倍以上(?)


        * 產生新的資料在Golang上面再也不會成為整個流程的瓶頸，網路處理速度也變快．



      * 目前他們也在思考把其他的部分開始改成Golang




  * [StatHat](http://www.stathat.com/)



    * **語言轉換:**



      * 一開始似乎就是用Go打造（99%）有部分也從RoR轉換到Go



    * **這是什麼?**



      * [StatHat](http://www.stathat.com/)是一個記錄資料的服務，他提供多個語言的支援（接近15個）．可以提供給其他網路服務公司做一些資料的追蹤， 可以顯示各種的圖形與方式．



    * **Golang帶給[StatHat](http://www.stathat.com/)的好處:**



      * 提了幾個優點



        * Fast:



          * 速度比較上，Golang在網路處理上算是真的相當快速的．



        * Resource Friendly



          * 他指的比較偏向社群資源多



        * Easy to deploy



          * Go binaries are self-contained


          * No shared libraries, no gem and dependency



        * Fun



      * 這邊有他們[Open Source出來跟他們產品有關的code](http://www.stathat.com/src)，可以參考．




  * [Iron.io](http://www.iron.io/)



    * **語言轉換:**



      * Ruby -> Golang



    * **這是什麼？**



      * [Iron.io](http://www.iron.io/) 是一個網路服務提供商，主要分成以下幾個產品:



        * ironWorker 平行的工作平台，本來用RoR寫但是有點慢，用Go改寫後就變快很多．


        * ironMQ 訊息序列傳送廠商，是一個可以擴充的服務




    * **Golang帶給[Iron.io](http://www.iron.io/) 的好處？**



      * 在ironWorker裡面很大一部份的工作是需要分配工作與平行處理架構，這是Golang主要能提升的部分，而他們會使用Golang的原因如下：



        * 速度快



          * 執行快而且編譯也快



        * Still reasonably express


        * Rich standard library



          * 也就是因為build-in 標準函示庫是足夠使用的，所以容易散佈跟編譯．



        * Not tie to VM



          * 指的是編譯好之後可以直接執行，不用綁太多環境．



        * Awesome



          * 寫起來很開心(我目前也是感覺相當有趣...)






  * **Q&A: (標記的公司為回答的代表)**



    * [Heroku] 如何決定哪些產品要轉換到Golang?



      * [回答]: 在把線上產品轉換前，其實有些利用Go來開發一些小元件．之後才決定要轉換，雖然每個人都覺得要重寫所有的元件是相當麻煩，但是.... 這其實有一點是個人決定（感覺是高層決定？）不過整體而言速度與開發時間讓人覺得很值得，



    *  [StatHat] 比起Go而言，Ruby 有著龐大的資源與社群那為什麼要轉換到Go呢?



      *  要用Go重寫其實是不難的（指的是他們用到的部分）



    * [Heroku][Juju] 你最喜歡的Go library



      * 不用懷疑～當然是Go fmt ． （我也覺得Go fmt幾乎把所有功能都用到了～)



    * [StatHat][Juju]  你們覺得學習Golang 要多久呢？



      * [StatHat] 我們大概學了幾個禮拜


      * [Juju] 我認為我從來不曾停止學習，能上手是很快但是要能真正了解其中含義，就要不斷學習．(這讓我想到這篇[文章](http://techorange.com/2014/07/09/step-step-path-becoming-great-software-developer/))



    * [StatHat] 你有任何推薦的Go書籍嗎？



      * Golang.org? 其實我並沒有讀任何的Golang 的書籍． 






**其他關於Golang 與其他語言的比較**






  * [http://www.slideshare.net/weijr/coscup2013](http://www.slideshare.net/weijr/coscup2013) [COSCUP2013] Python, F#, Golang and Friends


