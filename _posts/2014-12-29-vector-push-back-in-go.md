---
layout: post
layout: post
title: "[Golang]關於Slice在Append的一些發現"
description: ""
category: 
- golang
tags: []

---


![image](http://golang.org/doc/gopher/doc.png)

**前言:**

當初主要是[RUST](http://doc.rust-lang.org/nightly/intro.html#ownership)在官網上提出他們可以避免掉std:vector.push_back造成的referene 問題． 於是我也很好奇，究竟Golang會怎麼樣處理這樣的問題．  也由於Go本身把vector拿掉，我們就用Slice來試試看．


**筆記:**

![image](http://blog.golang.org/go-slices-usage-and-internals_slice-struct.png)

Slice 本身的結構包含資料本身，長度(len)與容量(cap)，根據[Slice Internal Document](http://blog.golang.org/go-slices-usage-and-internals#TOC_4.)裡面提到的．而且操作slice的append的時候，會根據你目前的容量(cap)來決定是否要重新分配記憶體位置．

比起std::vector好的是，如果記憶體重新分配了，原先參照的參數不會得到空的記憶體位置，而會變成是複製的內容． 但是要注意的是，參照的變數就不會跟著變動了． 這聽起來就是比較安全一點，不過程式不會造成pointer to null並不代表比較好，因為在這種狀況下．所參照的數值已經被修改，你可能會遇到更不能預測的問題．

所以比較優雅的方式，還是事先要保留足夠的資料．避免append造成的資料重新建立．


**奇怪的狀況，cap增長的大小根據compiler的不同**

我本來是很清楚這樣的狀況，直到我把我以下的程式跑在本地端(Mac/Win)還有遠端的[Golang Playground](http://play.golang.org/p/T2cotKNMHO)發現結果不一樣了．

是的！ 靚過第一次slice append之後，cap(slice)增長的數值竟然不一樣？？  (Playground是2，本地端是1) 一樣的狀況．類似的操作在c++ std::vector 就沒看到 (Win/Mac/Coliru online compiler) 增加過後的的cap都維持為"1". 這裏有參考的[C++程式碼](http://coliru.stacked-crooked.com/a/8f221d79cab13d87).  

於是我在[Stackoverflow](http://stackoverflow.com/questions/27665669/is-pointer-to-slice-using-reference-or-copy)詢問了這樣的問題，也引起了一些人的注意，幫我把[問題發到了Golang](https://github.com/golang/go/issues/9458)上面．雖然沒有辦法說服大家把這樣類似的問題放入spec或是warning，但是真的提醒大家，真正在使用slice的時候，必須像使用std::vector一樣的小心．最好的方式是不要參照，如果真的要參照的話，也建議使用保留的slice大小來避免記憶體重新的分配所造成的參照失效．


以下是完整的Golang 測試程式碼

<pre class="prettyprint">  
package main
 
import "fmt"
 
func main() {
	var a []int
	var b []int
	fmt.Printf("a len=%d, cap=%d \n", len(a), cap(a))
	a = append(a, 0)
	b = append(b, 0)
	p := &a[0]
	
	fmt.Printf("a[0] = %d pointer=%d, len=%d, cap=%d, p = %d \n", a[0], &a[0], len(a), cap(a), *p)
	a[0] = 2
	fmt.Printf("a[0] = %d pointer=%d, len=%d, cap=%d, p = %d \n", a[0], &a[0], len(a), cap(a), *p)
	/*
		a[0] = 0, p = 0
		a[0] = 2, p = 2
	*/
	var c []int
	var d []int
	fmt.Printf("c len=%d, cap=%d \n", len(c), cap(c))
	c = append(c, 0)
	d = append(d, 0)
	p2 := &c[0]
	fmt.Printf("a[0] = %d pointer=%d, len=%d, cap=%d, p2 = %d \n", c[0], &c[0], len(c), cap(c), *p2)
	c = append(c, 1)
	c[0] = 2
	fmt.Printf("a[0] = %d pointer=%d, len=%d, cap=%d, p2 = %d \n", c[0], &c[0], len(c), cap(c), *p2)

	/* 
		c[0]=0, p2 = 0
		c[0]=2, p2 = 0
	
	  in http://play.golang.org/p/DuClFZt2cK, the cap increasement by machine dependency.
		c[0]=0, p2 = 0
		c[0]=2, p2 = *2*
	  
	*/
}
</pre>
