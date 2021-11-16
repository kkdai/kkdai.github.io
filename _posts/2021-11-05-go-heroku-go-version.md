---
layout: post
title: "[學習心得][Golang] 在 Heroku 上使用 go-pg 會發生 undefined: sql.NullTime 錯誤的解決方式"
description: ""
category: 
- 學習文件
- TodayILearn
tags: ["Golang", "TIL"]
---

![image-20211107005949431](../images/2021/image-20211107005949431.png)



## 前言：

原來 Heroku 上面 #Golang  的版本需要有特殊 define 才會正確的讀取到。不然都會使用 1.12。

最近在改一隻 [LINE Bot](http://www.evanlin.com/go-ptt-bot/) 把原來已經不在 Heroku 支援的 MongoDB 改成 PostgreSQL ，想幫他加上免費的 PostgreSQL 但是遇到一些問題。先寫一下相關的學習。



## Golang + ORM = Go-PG

先挑選了一個套件是 

#### [https://github.com/go-pg/pg](https://github.com/go-pg/pg)

但是寫完後，發現 Local 都可以 compile ，但是丟到 Heroku 都會爆掉。



## error `undefined: sql.NullTime #59`

```
remote: # github.com/go-pg/pg/v10/orm
remote: vendor/github.com/go-pg/pg/v10/orm/table.go:41:40: undefined: sql.NullTime
remote: gopkg.in/mgo.v2/internal/scram
```

根據以下的 issue  [https://github.com/guregu/null/issues/59](https://github.com/guregu/null/issues/59) 解決方法就是只要升級到 go1.13 就好

```
go1.13 就好
go1.13 就好
go1.13 就好
```



## 強制讓 Heroku 使用更新版本 ( > Go 1.12 ) 的版本

阿勒～～～我的 Go local 不是已經升級到 1.17.2 了嗎？ 怎麼會？

```
remote:        Detected go modules via go.mod
remote: -----> 
remote:        Detected Module Name: github.com/kkdai/linebot-ptt-beauty
remote: -----> 
remote:  !!    The go.mod file for this project does not specify a Go version
remote:  !!    
remote:  !!    Defaulting to go1.12.17
remote:  !!    
remote:  !!    For more details see: htxtps://devcenter.heroku.com/articles/go-apps-with-modules#build-configuration
remote:  !!    
remote: -----> Using go1.12.17
remote: -----> Determining packages to install
```

問題來了.... 

不論你的 `go.mod` 上面的 Golang 版本有多新， Heroku 還是會使用 `go 1.12`

# force heroko to use go > 1.12

參考這個 stackoverflow 

[https://stackoverflow.com/questions/56968852/specify-go-version-for-go-mod-file](https://stackoverflow.com/questions/56968852/specify-go-version-for-go-mod-file)

```
module somemodule

// +heroku goVersion go1.14
go 1.14

require (
    // ...
)
```



如果你要使用最新版本的 Go 1.17.2 就改成

```
// +heroku goVersion go1.17
go 1.17
```

這樣就行了。

# 其他鏈結

- https://github.com/go-pg/pg/issues/445
- https://pg.uptrace.dev/
- https://devcenter.heroku.com/articles/getting-started-with-go?singlepage=true
