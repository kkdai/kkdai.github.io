---
layout: post
title: "[Golang] 關於 LINEBot Twitter 的流程"
description: ""
category: 
- TodayILearn
tags: ["TIL", "golfing", "crawler"]

---

## ![img](https://github.com/kkdai/twitter-auth-web/raw/master/images/heroku_setting.jpg)

# 開啟 Heroku 的 PostgreSQL 支援

- 參考 [https://devcenter.heroku.com/articles/heroku-postgresql](https://devcenter.heroku.com/articles/heroku-postgresql)

- 啟動方式參考以下，其中 `hobby-dev` 是基本免費的方式。 參考 [price plan](https://devcenter.heroku.com/articles/heroku-postgres-plans#hobby-tier)

  ```
   $ heroku addons:create heroku-postgresql:hobby-dev
  ```

- 這時候會自動把 PQSQL 的設定寫道 `DATABASE_URL` 裡面，你只要有以下的程式設定即可。

```
import	"github.com/go-pg/pg/v10"
....
		options, _ := pg.ParseURL(dbURL)
		db := pg.Connect(options)
		meta.Db = db
		defer db.Close()

		err = createSchema(db)
		if err != nil {
			panic(err)
		}

```





## 相關連結

- 

  
