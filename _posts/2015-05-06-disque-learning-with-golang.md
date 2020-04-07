---
layout: post
layout: post
title: "[Golang]學習disque(一)"
description: ""
category: 
- golang
tags: []

---

![image](http://gwb.blob.core.windows.net/shaunxu/Windows-Live-Writer/Redis-on-Windows_95D0/image_18.png)
(pic: source from [here](http://geekswithblogs.net/shaunxu/archive/2012/04/27/redis-on-windows.aspx)) 

以上的圖是利用Redis 達到 Message Queue 的方式，也是Disque要達到的事情．

##前言 

這一兩個月，比較常聽到大家討論的就是Disque的使用方式與疑問．本來我對於Message Queue的系統(尤其是backend那一塊)比較不熟．於是還是花了一點時間把[Disque](https://github.com/antirez/disque)裝起來，並且把sample code跑了一下．希望對於基本架構有一些了解．


##關於Disque

[Disque](https://github.com/antirez/disque) 是 [Redis](http://redis.io/)原作者[Salvatore Sanfilippo](https://github.com/antirez)根據大家在Redis上面針對Message Queue處理的部份來加強，並且下去拿Redis的source code加以修改，改造出這套專門處理Message Queue的系統．

主要特色如下: (參考[這裡](http://www.infoq.com/cn/news/2015/04/Disque-Redis-github))

- 消息發送可以選擇至少一次或者最多一次。
- 消息需要消費者確認。
- 如果沒有確認，會一直重發，直至到期。確認信息會廣播給擁有消息副本的所有結點，然後消息會被垃圾收集或者刪除。
- 隊列是持久的。
- Disque默認只運行在內存裡，持久性是通過同步備份實現的。
- 隊列為了保證最大吞吐量，不是全局一致的，但會儘力提供排序。
- 在壓力大的時候，消息不會丟棄，但會拒絶新的消息。
- 消費者和生產者可以通過命令查看隊列中的消息。
- 隊列盡力提供FIFO。
- 一組master作為中介，客戶端可以與任一結點通信。
- 中介有命名的隊列，無需消費者和生產者干預。
- 消息發送是事務性的，保證集群中會有所需數量的副本。
- 消息接收不是事務性的。
- 消費者默認是接收時是阻塞的，但也可以選擇查看新消息。
- 生產者在隊列滿時發新消息可以得到錯誤信息，也可以讓集群非同步地複製消息。
- 支持延遲作業，粒度是秒，最久可以長達數年。但需要消耗內存。
- 消費者和生產者可以連接不同的結點。

###優缺點與比較:

優點其實蠻容易被瞭解的:

-  容易安裝使用，而且小．本身就有資料庫(類似 redis)
-  當不需要太複雜的傳輸格式與介面的時候，disque效能應該不差 (base on [Redis v.s. RabbitMQ](http://devres.zoomquiet.io/data/20110714104018/index.html))

幾個缺點，可能需要注意:

- disque 目前還是alpha
- disque 目前只有單線程

關於與[Kafka](http://kafka.apache.org/)的比較，[Salvatore Sanfilippo](https://twitter.com/antirez)在他的推特有以下有趣的[回應](https://twitter.com/antirez/status/577444974331092992):      
 
        Salvatore Sanfilippo: Disque VS Kafka is the new Redis VS PosgreSQL which was the new Apple VS Orange :-)

##安裝與使用

###安裝與使用Disque

- git clone https://github.com/antirez/disque
- make
- make test  (make sure port and compiling result success.)
- cd src
- run disque-server


###簡單的操作

當你跑`disque`之後，就會自動連接local disque server．

        //Add a job by producer
        ADDJOB job1 "This is sample job1" 0   #create job1 with comment
        ->DI3b2954204f8f86168198266221515fb011a1eea005a0SQ #server response task id.
        
        //Get a job by consumer
        GETJOB from job1
        ->1) 1) "job1"
        ->2) "DI3b2954204f8f86168198266221515fb011a1eea005a0SQ"
        ->3) "This is sample job1"



### 透過Golang 來測試job-queue

主要是使用[go-disque](https://github.com/EverythingMe/go-disque/)的source code 來當作job-queue 的機制．

以下有三段程式碼，分別是兩個worker跟一個發送者，發送者(disque-enque)會發送兩個訊息排程給disque，當worker上線或是開始抓取訊息後，就會執行該訊息定義的事項．

主要程式碼並沒有做太大修改：

**Worker(Consumer 消費者):** Downloader : 會下載指定網頁的資料

        package main
        
        import (
        	"github.com/EverythingMe/go-disque/tasque"
        
        	"crypto/md5"
        	"fmt"
        	"io"
        	"net/http"
        	"os"
        	// "time"
        )
        
        // Step 1: Define a handler that has a unique name
        var Downloader = tasque.FuncHandler(func(t *tasque.Task) error {
        
        	u := t.Params["url"].(string)
        	res, err := http.Get(u)
        	if err != nil {
        		return err
        	}
        	defer res.Body.Close()
        
        	fp, err := os.Create(fmt.Sprintf("/tmp/%x", md5.Sum([]byte(u))))
        	if err != nil {
        		return err
        	}
        	defer fp.Close()
        
        	if _, err := io.Copy(fp, res.Body); err != nil {
        		return err
        	}
        	fmt.Printf("Downloaded %s successfully\n", u)
        
        	return nil
        
        }, "download")
        
        // Step 2: Registering the handler and starting a Worker
        
        func main() {
        
        	// Worker with 10 concurrent goroutines. In real life scenarios set this to much higher values...
        	worker := tasque.NewWorker(10, "127.0.0.1:7711")
        
        	// register our downloader
        	worker.Handle(Downloader)
        
        	// Run the worker
        	worker.Run()
        
        }

**Worker(Consumer 消費者):** foo : 顯示指定字串

        package main
        
        import (
        	"fmt"
        	"github.com/EverythingMe/go-disque/tasque"
        )
        
        // Step 1: Define a handler that has a unique name
        var fooWorker = tasque.FuncHandler(func(t *tasque.Task) error {
        	u := t.Params["text"].(string)
        	fmt.Printf("foo worker runs, param is %s\n", u)
        	return nil
        
        }, "foo")
        
        // Step 2: Registering the handler and starting a Worker
        
        func main() {
        
        	// Worker with 10 concurrent goroutines. In real life scenarios set this to much higher values...
        	worker := tasque.NewWorker(10, "127.0.0.1:7711")
        
        	// register our downloader
        	worker.Handle(fooWorker)
        
        	// Run the worker
        	worker.Run()
        
        }

最後就是把工作派發出去的Enque (角色上被稱為是**製造者 Producer**)

    package main
    
    import (
    	"fmt"
    	"github.com/EverythingMe/go-disque/tasque"
    	"time"
    )
    
    func main() {
    
    	client := tasque.NewClient(5*time.Second, "127.0.0.1:7711")
    	task := tasque.NewTask("download").Set("url", "http://google.com")
    	err := client.Do(task)
    	if err != nil {
    		panic(err)
    	}
    	fmt.Println("First work queue run..")
    
    	client = tasque.NewClient(5*time.Second, "127.0.0.1:7711")
    	task = tasque.NewTask("foo").Set("text", "I am the kind of world.")
    	err = client.Do(task)
    	if err != nil {
    		panic(err)
    	}
    	fmt.Println("Second work queue run..")
    }

##心得

Disque善用了Redis的特性，並且幫大家把一些基本功能都勾勒出來．簡單講就是看到太多人把Messagq Queue弄在上面，原作者才會這樣改．  

不過由於我本身用到的地方比較少，我相信有機會要使用的時後應該會更容易上手才對．


##相關文章

- [Golang client for Disque, the Persistent Distributed Job Priority Queue](https://github.com/goware/disque)
    - 另外一個Golang上面的disque client.
- [Adventures with Disque](http://geeks.everything.me/2015/05/03/adventures-with-disque/)
    - 關於disque 的評論也把幾個disque的client拿出來比較了一下．
- [Disque: Disque 使用教程](http://disquebook.com/)
    - 把安裝與使用教學翻譯成中文.. 相當有用
- [Geek News: Disque：Redis作者新开发的消息队列](http://geek.csdn.net/news/detail/27638)
    - 中文新聞介紹disque    
- [Redis作者谈Redis应用场景](http://blog.nosqlfan.com/html/2235.html)    
- [Redis能干啥？细看11种Web应用场景](http://os.51cto.com/art/201107/278292.htm)
- [Hacker News: disque](https://news.ycombinator.com/item?id=9447185)
- [李会军•宁静致远 | Redis作为消息队列与RabbitMQ的性能对比](http://devres.zoomquiet.io/data/20110714104018/index.html)
- [書籍: 使用Redis实现任务队列](http://book.51cto.com/art/201305/395461.htm)
    
