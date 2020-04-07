---
layout: post
layout: post
title: "[Golang] 學習Google Code Search 使用的Trigram Indexing" 
description: ""
category: 
- golang
tags: [go, algorithm]

---

![](http://img.rfclipart.com/image/preview/63-bd-a1/cloud-and-loupe-binary-code-search-Download-Royalty-free-Vector-File-EPS-18250.jpg)

## 前言

一直以來，我有在追蹤許多優秀工程師的github(RD的臉書)．其中尤其是[dgryski](https://github.com/dgryski)． 因為他有許多有趣使用Golang來開發的演算法小專案，所以我也會一起學習一些演算法與特別的資料結構．

本週的課程是[Trigram Indexing](http://ii.nlm.nih.gov/MTI/Details/trigram.shtml)．一邊學習，一邊寫成小專案．

## 什麼是Trigram Indexing

直接打Trigram會找到一堆關於卦的資料．不過Trigram主要是由兩個字組合而成 Tri-gram 也就是三個字元（[N-gram](https://en.wikipedia.org/wiki/N-gram)中的tri-gram)．

其實Trigram很簡單，主要就是把一串文字透過三個三個來分組:

- 將所有字元改成小寫
- 把每個空白處理．這裡有些不太相同，不少人將空白作為分隔符號．而Google Code Search把空白當作其中字元放進去．
- 把字元做成trigram

#### 如何做把一個單字做 Trigram 拆解

舉例:  Search

- 變小寫 "search"
- 開始拆解，三個三個為一組
	- sea
	- ear
	- arc
	- rch

#### 如何在程式裡實作拆解trigram

其實在程式裡面實作拆解很簡單，只是重要的是要如何比對．因為如果你真的把`"sea"`， `"ear"` .... 存成字串，比對又是相當的消耗時間．所以不論是Google還是一般人在做Trigram的時候都會這樣拆解．

講一個個字元換成ascii的`uint32`並且透過位移方式存放．舉例而言`s := "abc"`就變成 `uint32(s[0]), uint32(s[1]), uint32(s[2])`也就是 97, 98, 99．並且透過位移存放．`97<<16 + 98<<8 + 99 = 6382179`． 這樣比較的速度就會快的更多，也很適合儲存．


```go
	var trigrams []uint32 //存放所有拆解好的trigram
	s := "abc" //要拆解的字
	
	for i := 0; i <= len(s)-3; i++ {
		//透過將每個字元轉換成uint的方式，並且透過位移方式存放
		t := uint(uint32(s[i])<<16 | uint32(s[i+1])<<8 | uint32(s[i+2]))
		trigrams = append(trigrams, t)
	}

	//最後結果 6382179
```

## 如何在Code Search中使用

這邊開始很建議打開Russ Cox關於[Google Code Search的介紹文章](https://swtch.com/~rsc/regexp/regexp4.html)，雖然主軸是Regex 不過是透過Trigram Indexing的方式．

如同前面提到的，由於這個是"Code Search" 所以`空白`本身相當的重要．也就必須要將空白當作一個字元來作為Trigram Indexing的來源．

請注意： **本文重點在於討論Trigram Indexing，所以原先在Google Code Search針對Regex處理的部分就不討論．**

### 透過簡單例子來了解

比如說，我們現在要輸入搜尋的是以下三段文字:

1. Google Code Search
2. Google Code Project Hosting
3. Google Web Search

#### 處理空白與加上Trigram Indexing

接下來，讓我們真的來處理一些字串，這裡結果就會有點複雜，所以我們只拿第一段文字舉例：

"Google Code Search"來做Trigram Indexing:

```
"Goo", "oog", "ogl", "gle", "le_", "e_c", "_Co", "Cod", "ode", "e_S", "_Se", "Sea", "ear", "arc", "rch"
```

#### 在每一個Trigram上加上Document ID

`Document ID`就是你原本是第幾段文字(以這裡為例子），當然隨著文件變大，有可能是檔案或是磁碟代號． 這裡將以上的每個資料都加上 `{1}`．

#### 將Trigram 文字轉成數字方便比對

就像之前提到，要一個個比對文字 "Goo"比對 "Goo" 其實在CPU上面是比較慢的．而且存放成文字也是比較佔空間．所以比較好的方式就是都轉成Ascii的方式:

		"Goo" = uint("G") uint("o") uint("o") = 71 111 111

並且透過位移轉換，將他放成同一段數字`uint32`

		71 << 16 + 111<< 8 + 111 = 4681583
		
如此一來，之後再也不用比對文字`"Goo"`而是比對事不是數字`4681583`．

#### 如果一份文件中出現重複的trigram

前面沒有提到，不過我們還是要處理可能會重複出現的trigram．比如說`"Gooood"`，就會拆解成`"Goo", "ooo", "ooo", "ood"`．其中就有出現兩個`"ooo"`． 所以針對這樣的時候，我們還需要記錄這個trigram在某個Document中出現的次數． 以這個例子而言:

```
	"Gooood"  = "Goo", "ooo", "ooo", "ood"
				=  4681583, 7303023, 7303023, 7303012
				= 4681583 -> {1, 1} //第一個doc 出現一次
				  7303023 -> {1, 2} //第一個doc 出現一次
				  7303012 -> {1, 1} //第一個doc 出現一次
```

可以透過這個方式來表示一個trigram indexing 的文件資料．

出現的次數可以作為trigram 刪除的時候參考，輸入的字串也必須要有複數個才能把該文件裡的trigram整個刪除．


#### 如何在儲存這樣的資料

接下來要解釋，如何將Trigram Indexing結果做儲存．主要的方式是透過Golang 的map

```go
	
	var trigramIndex map[uint32][]int
	
	// 這裡指的是透過 trigram -> document ID slice
	// 舉例而言: "Goo" 出現在 Document 1,2,3
	// "Goo"-> {1, 2, 3}，假設 "Goo" 在第一個文件出現兩次，第三個文件出現三次
	//      -> { {1,2}, {2,1}, {3,3} }
	// 而 "Goo" 會變成 uint32 4681583
	// 就存放成 trigramIndex[4681583]-> []int= { {1,2}, {2,1}, {3,3} }

```

#### 查詢 (Query)

透過以上的方式，將所有的文件（以這裡為範例就是三段文字)． 做好Trigram Indexing之後，並且全部存成一連串的Doc ID之後． 我們就要來開始進行一個簡單的查詢:

假設我們要查詢`"Code"`這個字出現在哪些文件中，處理的流程如下:

- `"Code"`拆解成 `"Cod"`跟`"ode"`
- 透過轉換變成`4419428` 與 `7300197`
- 透過我們儲存好的mapping table，可以找到 `4419428 -> {1,2}`還有 `7300197 -> {1, 2}`
- 講兩個結果做`"AND"`，也就是 `{1, 2} AND {1, 2} = {1,2}`
- 查詢結果該文字出現在文件1跟文件2
	

#### 範例程式

以下寫好我自己的[範例程式](https://github.com/kkdai/trigram)，大家有興趣可以進去參考看看．[https://github.com/kkdai/trigram](https://github.com/kkdai/trigram)	

## 相關鏈結

- [Google Code Search (using Go)](https://github.com/google/codesearch)
- [Trigram Algorithm](http://ii.nlm.nih.gov/MTI/Details/trigram.shtml)
- [https://github.com/dgryski/go-trigram](https://github.com/dgryski/go-trigram)
- [Russ Cox: Regular Expression Matching with a Trigram Index](https://swtch.com/~rsc/regexp/regexp4.html)
- [Approximate string-matching algorithms, part 2](http://www.morfoedro.it/doc.php?n=223&lang=en#SimilarityMetric)
