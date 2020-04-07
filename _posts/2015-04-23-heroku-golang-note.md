---
layout: post
layout: post
title: "[Heroku][Golang] 一些關於筆記關於 Heroku 與 Golang"
description: ""
category: 
- golang
- heroku
tags: []

---

給自己做個筆記....


## 關於 Heroku

以下指令都需要安裝Heroku [Toolbelt](https://toolbelt.heroku.com/)後登入Heroku. `heroku login`

- **查詢Server 目前的log**:  `heroku logs -t` 其他更多指令在[這裡](http://stackoverflow.com/questions/2671454/heroku-how-to-see-all-the-logs)
- **Heroku App/Worker crash如何重啟**: `heroku restart web.1` 更多[指令](http://stackoverflow.com/questions/2671454/heroku-how-to-see-all-the-logs)

- **如何在Heroku上面快速的rollback**: 到control panel 下 [activity] 選擇你要的change旁邊有[roll back to here].
- **Performance Status**: 可以透過add-on [Librato](https://elements.heroku.com/addons/librato)來監測你目前在Heroku的webapp.
  
## 關於Golang

- Golang 的scope是依照package，所以同目錄下面的檔案，只要package一樣都會被歸在一起．
- init() 跑的順序是依照 alphabet axxx.go bxxx.go ...... zxxx.go
- 關於go import 更多使用要[看這](http://golang.org/ref/spec#Import_declarations)（好像是FAQ)
    - import _ "xxx": 代表的是使用 Init 但是不把整個package 套進來(不會有compiler error if no use)
    - import . "fmt": 打所有function 不用加上namespace
    - import m "fmt": `fmt.Println` 變成 `m.Println`

##  參考資料

- [Stackoveflow: heroku - how to see all the logs](http://stackoverflow.com/questions/2671454/heroku-how-to-see-all-the-logs)
- [Heroku: What to do when your dyno/worker crashes?](http://stackoverflow.com/questions/2671454/heroku-how-to-see-all-the-logs)
- [Heroku: Releases and Rollbacks](https://blog.heroku.com/archives/2013/7/25/releases-and-rollbacks)






    
