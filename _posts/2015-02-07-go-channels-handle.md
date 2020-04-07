---
layout: post
layout: post
title: "[Golang]關於 Channels 的控制一些要注意的事項(一)"
description: ""
category: 
- golang
- 學習文件
tags: []
---


![image](http://talks.golang.org/2014/readability/ref.png)

### 前言

學習Golang 讓人最興奮的大概就是它內建的 Concurrency 的支援，並且相當的容易使用． 
但是最近在學習與使用的時候，發現發現 Goroutine 可以讓本來需要用到 40 秒的 Request 減少到1/3 左右的時間．進而可以進步到 5 秒左右．平常的應用上，幾乎已經離不開 Goroutine 與Channel。

但是最近遇到幾個例子，一開始覺得很不能理解，也不容易解決．也是讓我找了一些解決方式．

### 一般用法

Go Channel一般有兩種用法(當然有更多的使用方式可以用)，一個是把channel當成資料來傳遞，另外的方式只是單純把channel當成是signal來等待或是啟動新的thread


<pre class="prettyprint">  
// 同時要處理多個可以同步運行的運算，利用channel來回傳結果
c := make(chan int)  // Allocate a channel.
go func() {
    result : = do_some_cal()
    c <- result
}()

c2 := make(chan int)  // Allocate a channel.
go func() {
    result : = do_some_cal2()
    c2 <- result
}()

totalResult := <-c2 + <-c
fmt.Printf("result is %d"), totalResult)   


// 傳送訊號(signal)的方式
c := make(chan int)  // Allocate a channel.
go func() {
    list.Sort()
    c <- 1  // Send a signal; value does not matter.
}()
doSomethingForAWhile()
<-c   // Wait for sort to finish; discard sent value.
</pre>

### 陷阱出現了

以上兩個方式相當的簡單，也相當直覺來使用．但是如果要跑goroutine的條件不是必要呢？也就是說不一定要透過go routine來同步執行很多的結果呢？

<pre class="prettyprint">  
c := make(chan int)  

//並不一定會進入go routine來計算，也就是說channel會是為空的．
if needRoutine == true {
    go func() {
        result : = do_some_cal()
        c <- result
    }()
} else {
    c <- 0 //note this kind will not work, because channels need input value in go routine.
}

// 這裏將會變成deadlock，而無窮的等待．
totalResult := <-c * 3 + 5
fmt.Printf("result is %d"), totalResult)   
</pre>

上面的例子可以看到，當needRoutine為false的時候，由於不會去跑go routine，所以 <-c會變成是deadlock． 

此外，如果你試著在else 的地方自己加上 c <- 0，也是沒有任何作用．

所以尋找過後，有一些解決的方式可以參考．

### 解決方式

#### 由邏輯來解決

比較簡單方式，並且code上面比較不會那麼複雜的方式是．就是在Go routine裡面去做額外的處理．也就是都會進去Go routine而在Go routine中再做檢查而回傳．
<pre class="prettyprint">  
c := make(chan int)  

go func() {
     //在Go Routine中處理
     if needRoutine == false {
        c <- 0 //or other invalid value
        return
     }
     result : = do_some_cal()
     c <- result
}()

// 這樣就不會變成無窮等待
result := <-c
if result > 0 {
   totalResult := result * 3 + 5
   fmt.Printf("result is %d"), totalResult)   
}
</pre>

#### 透過selector

透過[Selector](http://golang.org/ref/spec#Selectors)其實可以處理的事情相當的多． 它不僅僅可以分派處理很多個需求，先講解一些方式．

##### 透過timer

透過時間檢查， 過了特定時間就放棄不等． 這種解法比較好，只要在特定時間內如果能收到數值，即時還是可以順利得到結果．

<pre class="prettyprint">  
package main

import "time"
import "fmt"

func main() {

    c1 := make(chan string, 1)
    go func() {
        time.Sleep(time.Second * 2)
        c1 <- "result 1"
    }()
    
    select {
    case res := <-c1:
        fmt.Println(res)
    case <-time.After(time.Second * 1):
        fmt.Println("timeout 1")
    }
}
</pre>


##### 透過檢查channel有沒有設定過

以下例子可以檢查，如果channel有沒有設定過，才決定要不要拿來用． 不過這種狀況下，如果你的Goroutine需要一點時間無法馬上傳回解答的時候，這個時候就會馬上離開．

所以，如果原本到這個時候就應該有解答（不需要任何等待(non block))，可以使用這種方式．不然的話還是建議使用timer的方式．


<pre class="prettyprint">  
package main

import "time"
import "fmt"

func main() {

    c1 := make(chan string, 1)
    
    if someCondition == true {     
        go func() {
            time.Sleep(time.Second * 2)
            c1 <- "result 1"
        }()
    }

   select {
    case x, ok := <-c1:  //如果someCondition == true 除非這時候剛好得到結果，不然跑不到．
        if ok {
            fmt.Printf("Value %d was read.\n", x)
        } else {
            fmt.Println("Channel closed!") //Channel 被close.
        }
    default:
        fmt.Println("No value ready, moving on.") //Channel 沒有設定過，會馬上離開....
    }
}
</pre>
Refer [here](http://stackoverflow.com/questions/3398490/checking-if-a-channel-has-a-ready-to-read-value-using-go).

##### 進階的用法
這裏附上另外一個select的用法，就是來取得多個channel的資料．並且也用一個timer來顯示時間經過了幾秒． 全部做完才會離開.... 不然就是超過一定時間也會離開，避免有dealock產生．

<pre class="prettyprint">  
package main

import "time"
import "fmt"

func main() {

	c1 := make(chan string, 1)
	
	someCondition := true
	if someCondition == true {
		go func() {
			time.Sleep(time.Second * 4)
			c1 <- "result 1"
		}()
	}
	
	c2 := make(chan string, 1)
	if someCondition == true {
		go func() {
		    //故意延遲九秒，所以這個不會順利結束．
			time.Sleep(time.Second * 9)
			c2 <- "result 1"
		}()
	}
	
	doneCount := 0
	allDone := 2
	timeCount := 0
	
	// 別忘了... select 滿足任何一個都會離開，所以要有個for在外面讓他不停跑
	for doneCount < allDone && timeCount < 5 {
	
		select {
		//檢查C1
		case x, ok := <-c1:
			if ok {
				fmt.Printf("C1 in valus is %s.\n", x)
				doneCount++
			} else {
				fmt.Println("Channel closed!") //Channel 被close.
			}
		//檢查C2
		case x, _ := <-c2:
			fmt.Printf("C2 in valus is %s.\n", x)
			doneCount++
	
	    //另外準備一個離開條件，當五秒會離開...
		case <-time.After(time.Second * 1):
			fmt.Println("tick..")
			timeCount++
	
		}
	}
}
</pre>

[Go Play code is here](http://play.golang.org/p/8xYv2sX8VF)


#### 回過頭來看.. channel buffer..

關於channel沒經過goroutine會出問題的原因，經過尋找過後總算找到原因了． 可以參考以下[這篇文章](http://stackoverflow.com/questions/14050673/why-are-my-channels-deadlocking)．


    By default channels are unbuffered, meaning that they will only accept sends (chan <-) if there is a corresponding receive (<- chan) ready to receive the sent value.  (from [Go Example](https://gobyexample.com/channel-buffering))

也就是說，如果你沒有幫channel 設定好buffer (也就是 make(chan int, 1))先給他個預設空間的話．  channel <- value 會一直等到.... <- channel 才會順利結束．  這也是如果你直接使用channel 而沒有進入goroutine或是給予buffer就會deadlock

<pre class="prettyprint">  
package main

import "fmt"

func main() {

	messages := make(chan string)
	
	messages <- "buffered" //會有deadlock，因為會停到直到跑到 <-messages
	messages <- "channel"  //會有deadlock，因為會停到直到跑到 <-messages
	
	fmt.Println(<-messages)
	fmt.Println(<-messages)
}

</pre>

正確的解法，就是要給予channel buffer

<pre class="prettyprint">  

package main

import (
	"fmt"
	"time"
)

func main() {
	c := make(chan int, 1) //Add buffer cause not deadlock. http://stackoverflow.com/questions/14050673/why-are-my-channels-deadlocking
	needRoutine := false

	if needRoutine {
		go func() {
			time.Sleep(time.Second * 3)
			c <- 3
		}()
	
	} else {
		c <- 0
	}
	
	totalResult := <-c*3 + 5
	
	fmt.Printf("result is %d", totalResult)
}
</pre>

Go play is [here](http://play.golang.org/p/UcKXoX0IR2)

### 參考資料

- [Effective Go: Channels](https://golang.org/doc/effective_go.html#channels)
    - 官方Effective Go文件，一定要熟讀．
- [Concurrency, Goroutines and GOMAXPROCS](http://www.goinggo.net/2014/01/concurrency-goroutines-and-gomaxprocs.html)
    - 算是簡介文章與範例說明....
- [Go by Example: Timeouts](https://gobyexample.com/timeouts)    
    - Go by example 是最好學習Go的方式，並且搭配著[Go Play](http://play.golang.org/)可以馬上看結果．
- [Stackoverflow : Checking if a channel has a ready-to-read value, using Go](http://stackoverflow.com/questions/3398490/checking-if-a-channel-has-a-ready-to-read-value-using-go)    
- [Slideshare: Go for the would be network programmer](http://www.slideshare.net/feyeleanor/go-for-the-would-be-network-programmer)
- [Slideshare: RubyConf Portugal 2014 - Why ruby must go!](http://www.slideshare.net/gautamrege/rubyconf-portugal-2014-why-ruby-must-go)
