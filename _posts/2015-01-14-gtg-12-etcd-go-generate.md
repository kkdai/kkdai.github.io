---
layout: post
layout: post
title: "[研討會心得]Golang Taipei Gathering #12 etcd in IoT /go generate"
description: ""
category: 
- 研討會心得
- golang
tags: []

---

![image](http://golang.org/doc/gopher/talks.png)

**前言:**

本來還在思考要不要參加，但是剛好gtg slack上面有人願意把票讓出來．就趕快來聽聽．此次的相關資料在[這裡](http://golang.kktix.cc/events/gtg12)．

**研討會筆記:**

- [POGA]Go Generate, trick to write 
    -  Ruby 
        - 適合拿來寫 DSL
        - Stirng/HASH Anywhere
        - lack of "syntax eror"
    - GO
        - sort 不支援int64 sort, 其實只是多寫成4 lines :). 
            - [參考這篇](https://groups.google.com/forum/#!topic/golang-nuts/tyDC4S62nPo)
            
                Because it's four lines of code:
                
                type int64arr []int64
                
                
                func (a int64arr) Len() int { return len(a) }
                
                
                func (a int64arr) Swap(i, j int} { a[i], a[j] = a[j], a[i] }
                
                
                func (a int64arr) Less(i, j int) bool { return a[i] < a[j] }
                
                Float64 is the more common float, int is the more common it, so both have predefined versions of this.
                
        - self-refererencial function
        - struct tag using reflect is slow, don't use it in DSL.
        - go generate 僅僅是regex來替換而非parser
            - "go get" don't run "go generate" for you
        - slide: [http://www.slideshare.net/poga/gtg12](http://www.slideshare.net/poga/gtg12)
        - 與講者額外的談話:
            - struct tag 可能會發生問題.. 由於json tag 是會被go compiler skip，但是極有可能會發生一些錯誤 
                - tag 內名稱寫錯而永遠讀不到json的內容
                - 不小心把一些語法寫在雙影號外面．(e.g. omit...)
                - 寫錯不會造成整筆資料都是空的，而是某個欄位會變成空的．這就比較麻煩．
            - 關於go generate 使用實例:
                - 把他當成shell script
                - 自我檢查，比如slide裡面的auto error
            
- [HAWK]miicasa etcd for IoT
    - What is IoT?
        - 物聯網 - 蒐集狀態機
            - 透過簡單的 AND OR NOT的來控制．
            - 舉例而言:
                - 設計兩個開關與一個燈泡，透過XOR來控制燈泡．可以把開關放在房間內與門口．
                - 類似 Key-Value的處理法
    - etcd
        - 簡單，安全，快與可信賴
            - 1000 per/s
        - 效能測試:
            - 56 kB 10萬條長連線， 15G Ram.
                - 5k 1.6s
                - 10k 2.4s
            - etcd heart bit
            - HTTP heart bit
            - 不同語言效能
                - C -> 50萬
                - Go -> 15萬                                 
        - demo:
            - 講者準備一個感知元件，當收到有人經過會去把燈泡做開關的動作．
            - 一個PIR(Peson In Rear) 的sensor搭配sender．一遇到信號就連上etcd去變更資料．
            - 一個類似smart plug的接受端，透過etcd的wait來等待信號的改變．
                - 直接用etcd wait (有keep alive)
                - Http本身也有keep alive，斷掉的話重新連線監聽
        - 心得：
        
            - etcd有cluster的概念，可以使用在個別的IoT設備上．其實kktix的小神器掃QRCode幾就已經把它整合redis進去，然後定期同步．
            - 講者的使用方式主要偏重於插電的IoT(Internet of Thing)，不過我認為在電池方面應該會更加的省電，因為使用http的鏈結電量應該遠遠比其他方式少．
            - 大陸方面也有流行跟微信接口的IoT，這樣就不需要自己架設相關的伺服器．jabberd2雖然不難架，但是要管很累．                 
                
       
