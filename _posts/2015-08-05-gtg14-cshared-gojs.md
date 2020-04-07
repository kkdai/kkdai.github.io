---
layout: post
layout: post
title: "[研討會心得][GTG14][Golang]第十四次的Golang Taiwan聚會"
description: ""
category: 
- golang
- 研討會心得
tags: []

---

## 前言

小孩出生後難得可以參加研討會[GTG#14](http://golang.kktix.cc/events/gtg14)，主要也是想看看Go 1.5的一些新功能介紹．

## 第一階段: tka - 用 Golang 幫 Ruby 加速


### 筆記:

- buildmode=c-shared:
    - 啟動版本: Golang 1.5 beta2 
    - 環境: linux / android  (win/mac 不支援)
- 資料結構不同:
    - GoInt8 -> signed char
    - GoString -> char *p, int length
- Ruby FFI:
    - ruby package to load dynamic libraries.

呼叫方式:

        require 'ffi'                                                                                                                           
        require 'benchmark'                                                                                                                     
                                                                                                                                                
        module LibGo                                                                                                                            
          extend FFI::Library                                                                                                                   
          ffi_lib './libgo.so'             
          //               (fn) (input) (output)                                                                                                     
          attach_function :fib, [:int], :int             
        end  


### 效能比較

- 簡單功能 Ruby > golang 五倍
- fibinacci Golang > Ruby 二十倍


### 參數挑選

- 使用GoString 與 使用C.String 
    - 結果:
        - C.String 會比較快
    - 原因:
        - Ruby 轉 Golang string 需要配置兩個參數 (char * 與 length)

###  Q&A

- C-Shared GC
    - Not tested yet.
- C++ binding with Go
    - 速度應該沒差    
    
### 其他:

有介紹到的[golang Language package](https://godoc.org/golang.org/x/text/language)看起來很好用    

##  第二階段: 用 Go 寫前端， GopherJS 優劣分析


### Slide:


<iframe src="//www.slideshare.net/slideshow/embed_code/key/4zal7qw30XE7C8" width="425" height="355" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/poga/fullstack-go-with-gopherjs" title="Full-stack go with GopherJS" target="_blank">Full-stack go with GopherJS</a> </strong> from <strong><a href="//www.slideshare.net/poga" target="_blank">Poga Po</a></strong> </div>


## 筆記

** Fullstack Go**

- GopherJS
    - Goroutines works in JS
- JS s signle-thread
    - GopherJS provide roroutine scheduler.

### Examples:

- DOM
- Callback

#### 效能:

測試五千個goroutine來選轉圖片是可以接受．

### Gocha

- No dead code elimation
    - Use js not fmt to reduce size.
- Callback should not block
    - JS is single thread. (main thread)

## 會後心得:

- 使用brew 來管理Go版本，好像很酷

## 參考資料:

- Golang Shared:
    - [https://gist.github.com/tka/bab2e516f125511de67e](https://gist.github.com/tka/bab2e516f125511de67e)
    - [http://qiita.com/yanolab/items/1e0dd7fd27f19f697285](http://qiita.com/yanolab/items/1e0dd7fd27f19f697285)
- GopherJS
    - [https://github.com/gopherjs/gopherjs](https://github.com/gopherjs/gopherjs)    
- [Go Build ccurrent support](https://github.com/golang/go/blob/ae3e3610d5ea9814fcc8bff5c4cea51795465565/src/cmd/go/build.go#L326)

- [Golang瀏覽器語系挑選與比對](https://godoc.org/golang.org/x/text/language)    
