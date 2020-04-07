---
layout: post
layout: post
title: "[Golang]關於Go的Constant的型別轉換(Type Cast)"
description: ""
category: 
- golang
tags: []

---

![image](https://golang.org/doc/gopher/talks.png)

## 前言:

原本的問題是出現在[Stackoverflow](http://stackoverflow.com/questions/28791568/compile-error-when-comparing-between-named-type-and-unamed-type)，有人發問template.HTML("test") == "test" 可以過，但是template.HTML("test") == some_str 卻不能成
功compiler．

## 原因:
主要的原因是由於Go 本身是static typing的程式語言，而constant本身的型別是未定，並且對於constant在compiler的狀態下是會有一些型別轉換的．

        // 型別尚未確定
        const hello_string = "Hello, 世界"
        
        // 型別確定為字串
        fmt.Printf(" type is %T \n",  hello_string)
        //type is string


## 範例程式碼:
根據[The Go Blog: Constants](http://blog.golang.org/constants) 裡面有詳細提到關於Go 是 statical typing的一些問題． 接下來用幾個範例解釋:

<pre class="prettyprint">  
package main

import (
	"fmt"
)

func main() {
	// hello Type is not define for now.
	const hello = "Hello, 世界"
	// Type is String
	const typedHello string = "Hello, 世界"

	// hello type is cast to string
	fmt.Printf("Type of hello is=%T, typedHello is=%T \n", hello, typedHello)
	// hello is non-type it convert to string and compare
	fmt.Println(hello == typedHello)

	//Another sample. Define MyString as a string type
	type MyString string
	var m MyString
	// m is non-type for now.
	m = "Hello, 世界"

	// It will introduce error, because m type is MyString and typeHello is string.
	//fmt.Println(m == typedHello)
	// hello is non-type until compare with m. It will define as MyString type.
	fmt.Println(m == hello)

}
</pre>

Go Play is [here](http://play.golang.org/p/WV6Ad6DuNA).
