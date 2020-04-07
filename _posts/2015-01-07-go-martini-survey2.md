---
layout: post
layout: post
title: "[Golang]關於martini架構的更深一步了解"
description: ""
category: 
- golang
tags: []

---

![image](http://i.imgur.com/lb8cQ3g.jpg?1)


**前言：**

最近開始深究Go的[webframework martini](https://github.com/go-martini/martini)．慢慢開始會寫更多的東西出來．


**筆記:**

以下敘述的方式，會根據我想要達成的目標依序紀錄一下：

- [JSON] 解決martini JSON的資料解析(parse)
    - 不論是Martini或是 http.Request原本基本都會是使用JSON.Unmarshall來處理．
    - 這裏需要注意的是，跟當初在處理mongodb的資料一樣．變數的名稱不可以全部是小寫，但是json資料名稱需要全部都是小寫．
    - *如果不小心把變數名稱全部打成小寫，會無法正確的Unmarshall資料．*

<pre class="prettyprint">  

type Response2 struct {
    Page   int      `json:"page"`
    Fruits []string `json:"fruits"`
}

str := `{"page": 1, "fruits": ["apple", "peach"]}`
    res := &Response2{}
    json.Unmarshal([]byte(str), &res)
    fmt.Println(res)
    fmt.Println(res.Fruits[0])
</pre>


- [martini-render] 關於martini 顯示json的顯示部份．原本[encoding/json](http://golang.org/pkg/encoding/json/)能處理大部分的事情．但是要能完整顯示http status code 與json內容的話．我還是選擇了[martini-contrib/render](https://github.com/martini-contrib/render)

<pre class="prettyprint">  
    render.JSON(400, map[string]interface{}{"result": "success"})
</pre>

- [Martini map]接下來問題是，如何在各個martini handler中，去傳遞你要的變數或是如何把資料庫加入．
    - 原本是應該要直接使用martini.Use(DB()) 然後再去接 DB 的起始martini.handler即可．[參考這裡](http://blog.gopheracademy.com/advent-2013/day-11-martini/)
    - 但是想要做成可變動的資料庫格式（in-mem DB, mongodb 共存) 所以還是直接丟整個變數比較好．[類似作法參考](https://github.com/go-martini/martini)

<pre class="prettyprint">  
var db SomeDB{} 
//將變數透過martini 傳到各個handler
m.Map(&db)
...


//新增一個變數參數就可以使用
func someHandler(params martini.Params, r render.Render, db *SomeDB) {
....
}
</pre>    

**相關文章:**

- [Go Doc: Init function](https://golang.org/doc/effective_go.html#init)
    - Init是負責每個檔案的起始function，會比main()更早啟動．但是各個檔案的Init並沒有一定順序．
    - 根據這份文件提到，他在整個程式中的順序會是:
        - 變數起始
        - Import
        - Init
- [Go :Methods on structs](http://golangtutorials.blogspot.tw/2011/06/methods-on-structs.html)        
- [Build a RESTful API with Martini](http://0value.com/build-a-restful-API-with-Martini)
- [Learning Go with Martini - Working with MongoDB](http://progadventure.blogspot.tw/2014/03/learning-go-with-martini-working-with.html)
- [Simple App with Go, Martini, Gorp and MySQL](http://techslides.com/simple-app-with-go-martini-gorp-and-mysql)
    
