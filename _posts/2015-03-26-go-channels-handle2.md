---
layout: post
layout: post
title: "[Golang]關於Channels的控制一些要注意的事項(二)"
description: ""
category: 
- golang
tags: []
---


![image](http://talks.golang.org/2014/readability/ref.png)

##前言

前一篇的一些討論後，接下來有一些更容易出錯的部分可以討論．主要focus Goroutine跟 defer


###Goroutine Closure

主要是[這一篇部落格](https://blog.cloudflare.com/a-go-gotcha-when-closures-and-goroutines-collide/)帶出的問題:

        func main() {
            done := make(chan bool)
        
            values := []string{"a", "b", "c"}
            for _, v := range values {
                go func() {
                    fmt.Println(v)
                    done <- true
                }()
            }
        
            // wait for all goroutines to complete before exiting
            for _ = range values {
                <-done
            }
        }

根據以上的部分，印出的結果不會是 "a", "b", "c"．而是 "c", "c", "c" 原因是 goroutine 變數會參照到go func 跑的時候．


如果修改成以下就可以避免這個問題：

        func main() {
        	done := make(chan bool)
        
        	values := []string{"a", "b", "c"}
        	for _, v := range values {
        		go func(obj string) {
        			fmt.Println(obj)
        			done <- true
        		}(v)
        	}
        
        	// wait for all goroutines to complete before exiting
        	for _ = range values {
        		<-done
        	}
        }

由於他的順序會是 `go func(v)` 之後才執行，所以其變數內容會先傳過去而不是跑道`fmt.Println(v)`才取得．  更多跟goroutinem與closure有關的資訊請看這裡[Go: FAQ](https://golang.org/doc/faq#closures_and_goroutines)



### 參考資料

- [Effective Go: Channels](https://golang.org/doc/effective_go.html#channels)
    - 官方Effective Go文件，一定要熟讀．
- [Go FAQ: hat happens with closures running as goroutines?](https://golang.org/doc/faq#closures_and_goroutines)
- [A Go Gotcha: When Closures and Goroutines Collide](https://blog.cloudflare.com/a-go-gotcha-when-closures-and-goroutines-collide/)
