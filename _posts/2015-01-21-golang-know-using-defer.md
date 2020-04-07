---
layout: post
layout: post
title: "[Golang] 相當好用但又要注意的defer"
description: ""
category: 
- golang
tags: []
---

![image](https://camo.githubusercontent.com/7908c3868124b4a1ea0e6e43ff7e7b9e62069815/687474703a2f2f676f6c616e672e6f72672f646f632f676f706865722f66726f6e74706167652e706e67)


- 經常在處理檔案的開關必須要注意到file open就一定要有搭配的 file close．但是如果有很多的case，搭配著很多的return．那是不是就得在每個地方寫上fclose?
<pre class="prettyprint">  

func FileProcess() error {
    //開啟檔案
    f, err := os.Create("/tmp/dat2")
    check(err)
    
    //在這裡呼叫 defer f.Close()，如果有參數會這時候讀入
    defer f.Close()
    
    if CASE1 {
        Do something        
        //這時候會執行 f.Close()
        return  errors.New("Error Case2")
    } else if CASE2 {
        Do something
        //這時候會執行 f.Close()
        return errors.New("Error Case2")
    } 
    // 就算之後要增加新的case，也不用擔心要補 f.Close()
    
    //這時候會執行 f.Close()
    return nil
}

</pre>

- Golang裡面有個很方便的function 叫做defer，他執行的時間點會是在你離開目前的function．
- 不過這裡需要注意的是有兩個地方:
    - 變數的傳遞，會在呼叫defer的時候傳入．所以他並不是很簡單的直接移到最後呼叫
        - 根據[GoDoc Defer](https://golang.org/doc/effective_go.html#defer): The arguments to the deferred function (which include the receiver if the function is a method) are evaluated when the defer executes.
    - 呼叫與執行defer採取的是LIFO．
- [參考這篇文章](http://blog.jgc.org/2015/01/failing-to-rtfm-for-go-defer-statements.html)
- 結論:
    - defer最好還是使用在fopen與fclose比較不會有問題．
    - 如果有參數要帶，千萬要想清楚當時參數的順序與是否有其他問題會發生．
    
<pre class="prettyprint">  

package main

import (
	"errors"
	"fmt"
)

func useAClosure() error {
	var err error
	defer func() {
		fmt.Printf("useAClosure: err has value %v\n", err)
	}()

	err = errors.New("Error 1")
	fmt.Println("Finish func useAClosure")

	return err
}

func deferPrintf() error {
	var err error
    //呼叫的時候會把參數帶過去，所以err是nil
	defer fmt.Printf("deferPrintf: err has value %v\n", err)

	err = errors.New("Error 2")

    //注意這裡是LIFO，所以呼叫會是43210
	for i := 0; i < 5; i++ {
		defer fmt.Printf("%d ", i)
	}
	fmt.Println("Finish func deferPrintf")
	return err
}

func main() {
	useAClosure()
	deferPrintf()
}

/* output
Finish func useAClosure
useAClosure: err has value Error 1
Finish func deferPrintf
4 3 2 1 0 deferPrintf: err has value <nil>
*/
</pre>
