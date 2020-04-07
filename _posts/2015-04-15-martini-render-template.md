---
layout: post
layout: post
title: "[Golang][Martini]在Martini-render上面的layout/template套用"
description: ""
category: 
- golang
tags: []

---

![image](http://i.imgur.com/lb8cQ3g.jpg)

## 前言:

學習架設Golang Web Services 的時候，也需要有後台網頁管理介面．這時候就需要有樣板來套．才能讓網站架設起來比較快．

## 筆記:

以下的部分會提到如何使用，我個人使用習慣． 不會將所有的程式碼都貼出來，但是會顯示部份的整理過的資料．

### 關於Martini的layout:

網頁上的資料部分，可以拆解成兩大部分:

- 不可以抽換，使用layout替換 (ex: `layout.tmpl`)
    - 舉例：  <HTML><HEAD> 甚至是CSS JS都可以．
- 需要用程式來表示的部分，用render來顯示 (ex: `hello_world.tmpl`, `layout_hello.tmpl`)
    - 舉例: table content或是資料庫裡面的資料．    

Layout file `layout.tmpl` 

        <!doctype html>  
        <html>  
        <head>  
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Layout sample</title>
        </head>
        
        <body style="margin: 20px;">  
        <h1>This is a layout sample</h1>  
        {{ yield }}  <!-- 可以被替換的部分放這裡 -->
        </body>  
        </html>  

其中 `{{ yield }}` 將你要顯示的tmpl部分全部換到這邊． 



接下來要處理的部分拆解到`hello_world.tmpl`.

        <CENTER> Hello {{ . }} </CENTER>

接下來另外一個需要處理的到`layout_hello.tmpl`.

        <CENTER> This is another layout Hello {{ . }} </CENTER>
        
接下來就要提到Golang處理layout的部分．

### 如何使用Layout:


提到Golang處理的部分，先看看在一開始martini啟動的部分


        .....
        func main() {
            m := martini.Classic()
            
            // 將所有預設的layout都先設定到"layout.tmpl"
            m.Use(render.Renderer(render.Options {
                Layout: "layout",
            }))
            .....
            m.Run()
        }

這邊要記住，預設的目錄都會在`templates`下面．


要顯示的時候可以使用以下的方式．

        // 使用hello_world.tmpl
          m.Get("/", func(r render.Render, db *mgo.Database) {
            display_name := "John"
            r.HTML(200, "hello_world", display_name)
          })  

### 如果不同的entry point 要使用不同的layout呢？

有一些時候，網站開始變大之後．layout 的版型也開始變多了．這時候可能需要動態的去變你要套用的layout．原本的martini-render 就提供可以在render.HTML裡面加上 `HTMLOptions`．其實也就是加上你要指定的layout．


        // 使用hello_world.tmpl，並且套上layout_hello.tmpl
          m.Get("/", func(r render.Render, db *mgo.Database) {
            display_name := "John"
            r.HTML(200, "hello_world", display_name, render.HTMLOptions{Layout: "layout_hello"})
          })  


## 相關鏈結:

- [GoDoc: martini-render](https://godoc.org/github.com/codegangsta/martini-contrib/render)
    - 這一篇是GoDoc不得不看..
- [GoDoc: template](http://golang.org/pkg/html/template/)
    - 關於http/template，不過martini-render是架設在他之上．可看可不看．
- [GO TUTORIAL WITH MONGODB ON HEORKU](http://blog.jeffdouglas.com/2014/07/25/go-tutorial-with-mongodb-on-heorku/)
    - 是一篇tutorial，不過對於martini-render有一些介紹．
- [Hack martini-render](http://libraries.io/go/github.com%2Frnoldo%2Frender)
    - 很推薦看這一篇，基本常識都會了解．
- [初學 Golang 30 天（二十八）Router, Template](http://ithelp.ithome.com.tw/question/10159689)
    - 中文的介紹文章    
- [Stackoverflow: html/template: “layout” is undefined](http://stackoverflow.com/questions/27468281/html-template-layout-is-undefined)
    - 容易遇到的問題
    
