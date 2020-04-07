---
layout: post
layout: post
title: "[Golang] 來玩玩Golang的效能評估-Benchmark"
description: ""
category: 
- golang
tags: [go, benchmark]
---

![](https://blog.golang.org/6years-gopher.png)

##前言

在寫[Project52](http://www.evanlin.com/52week-52projets/)的過程中，其實寫了不少的資料結構實作，或是寫了一些演算法的實作．  一直以來由於Golang內建了單元測試的工具，所以基本的`go test`都有跑．

不過最近由於有個小專案[trigram](https://github.com/kkdai/trigram)人氣飆高，所以心血來潮來跑跑他的效能測試．發現效果不太好，於是本週的課題就是要來評估你的Go專案效能，並且讓他跑得更快．

## Go內建的效能評估 -  Benchmark

###如何建立效能評估

首先要講回來，在Go裡面通常而言的習慣是我們會把一個相關的物件寫在同一個檔案．並且把相關的測試寫在`obj_test.go`裡面．舉例而言:

``` 
一個物件 skiplist
檔名為  skiplist.go
相關測試與效能評估會寫在  skiplist_test.go
```

那效能評估要怎麼寫，我拿個例子來看:

```go
func BenchmarkSliceInsert(b *testing.B) {
	var sl []uint32
	b.ResetTimer()
	var i uint32
	for i = 0; i < uint32(b.N); i++ {
		sl = append(sl, i)
	}
}
```

這段代碼主要是來效能評估slice對於append的速度．

###如何跑效能評估

那麼要如何在Go上面跑效能評估呢?  

```
go test -bench=.
```

就可以看到類似的效果

```
BenchmarkSliceInsert-4   	100000000	        29.6 ns/op
```

### 不同資料結構間的效能評估

接下來放一些關於我在測試skiplist的數據，也能做些簡單的筆記:

```
BenchmarkSliceInsert-4   	100000000	        29.6 ns/op
BenchmarkSliceSearch-4   	   20000	     81506 ns/op
BenchmarkMapInsert-4     	 5000000	       283 ns/op
BenchmarkMapSearch-4     	30000000	        42.4 ns/op
```

這邊有些簡單的重點可以整理:

#####  以資料結構插入而言: Slice 最快，Map 很慢 

這也是很重要的slice是簡單的資料結構，雖然要iterator起來相當的繁瑣．（也很慢） 但是就資料的新增操作上，就快很多了．

如果要寫會一直變動的資料結構（或是很多複製與新增刪除的動作）就比較建議還是使用slice

#### 以資料查詢而言: Map實在快多了

由於我測試流程裡面，slice是以找不到為範例．所以速度差異有點大．不過事實也是，Map的查詢真的快多了，所以如果要建立大量查詢的資料，其實很建議使用Map而不是使用Slice來管理你內部的資料存取．


## 小專案:

最後，我一樣放上我本週的專案．就是把上個禮拜的專案"[trigram](http://github.com/kkdai/trigram)"的效能提升後，並且把他的切割格式增加兩個與四個．修改為[Ngram](https://github.com/kkdai/ngram)


## 相關鏈結:

- [我的Golang Project 52文章](http://www.evanlin.com/52week-52projets/)
- [Golang- Package testing](https://golang.org/pkg/testing/)
- [DFC: How to write benchmarks in Go](http://dave.cheney.net/2013/06/30/how-to-write-benchmarks-in-go)
