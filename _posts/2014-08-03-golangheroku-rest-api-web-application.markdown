---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-08-03 15:32:28+00:00
slug: golangheroku-%e5%a6%82%e4%bd%95%e7%94%a8golang-%e5%81%9a%e5%87%ba%e7%b0%a1%e5%96%ae%e7%9a%84-rest-api-web-application
title: '[Golang][Heroku] 如何用Golang 做出簡單的 REST API Web Application'
wordpress_id: 1473
categories:
- Golang
- 學習文件
---

 網路上找了一下方法，其實方法相當的多．大家可以找自己適合的方式來做，反正我也在學，乾脆把每一種看到的都開始試試看：


<!--more-->




  * net/http



    * [這篇文章](http://dougblack.io/words/a-restful-micro-framework-in-go.html)談到如何用net/http 來做，其實也沒那麼難，不過問題都一樣．都得對於每個物件與方式座對應．


    * 不僅僅是個別東西得自己寫出來，由於每一個都是走 [url.values](http://golang.org/pkg/net/url/)所以幾乎是無法辨別 albums/1 這樣的REST API，必須傳遞 ?albums=1 


    * 目前還沒有想到比較好的方式可以做出get/add/delete，繼續研究其他的．



      * 參考:



        * [http://dougblack.io/words/a-restful-micro-framework-in-go.html](http://dougblack.io/words/a-restful-micro-framework-in-go.html)





  * [gorilla/mux](http://www.gorillatoolkit.org/pkg/mux):



    * 可以使用 gorilla/mux的方式來把 http.handle request 去route 到各個不同functions去．


    * [這裡](http://joshua.themarshians.com/hardcore-google-communicating-go.html)有詳細的方法，看不懂可以拿我寫好的[code](https://gist.github.com/kkdai/7a2edf58fcbbf5bc1ce4)去參考．不過只有list all，如果要做出 albums/1 這種狀況似乎是沒有方法的．


    * 跟net/http的問題是一樣的，也沒有熟悉到有想到解決方可以可以實作 item GET/PUT/POST/DELETE  (ex: album/1)



      * 參考：



        * [http://www.gorillatoolkit.org/pkg/mux](http://www.gorillatoolkit.org/pkg/mux)


        * [http://joshua.themarshians.com/hardcore-google-communicating-go.html](http://joshua.themarshians.com/hardcore-google-communicating-go.html)





  * [go/martini](https://github.com/go-martini/martini)



    * martini算是相當適合拿來做REST(基本上內建已經支援了)



      * 

    
    <span style="box-sizing:border-box;" class="nx">m</span><span style="box-sizing:border-box;" class="p">.</span><span style="box-sizing:border-box;" class="nx">Get</span><span style="box-sizing:border-box;" class="p">(</span><span style="box-sizing:border-box;color:#dd1144;" class="s">"/hello/:name"</span><span style="box-sizing:border-box;" class="p">,</span> <span style="box-sizing:border-box;font-weight:bold;" class="kd">func</span><span style="box-sizing:border-box;" class="p">(</span><span style="box-sizing:border-box;" class="nx">params</span> <span style="box-sizing:border-box;" class="nx">martini</span><span style="box-sizing:border-box;" class="p">.</span><span style="box-sizing:border-box;" class="nx">Params</span><span style="box-sizing:border-box;" class="p">)</span> <span style="box-sizing:border-box;color:#445588;font-weight:bold;" class="kt">string</span> <span style="box-sizing:border-box;" class="p">{</span>
      <span style="box-sizing:border-box;font-weight:bold;" class="k">return</span> <span style="box-sizing:border-box;color:#dd1144;" class="s">"Hello "</span> <span style="box-sizing:border-box;font-weight:bold;" class="o">+</span> <span style="box-sizing:border-box;" class="nx">params</span><span style="box-sizing:border-box;" class="p">[</span><span style="box-sizing:border-box;color:#dd1144;" class="s">"name"</span><span style="box-sizing:border-box;" class="p">]</span>
    <span style="box-sizing:border-box;" class="p">})</span>






    * 這邊要注意的是：



      * 內容是  :name 你可不能輸入 curl  -i  localhost.com:xxxx/hello/:**name  **而必須要輸入 name


      *  params[“name”] 出來都是字串，要轉數字要注意error exception.  


      * 其實有個文件上沒有講清楚的就是針對 Get/PUT/POST 的function input，少寫是會找不到的  
func getItem(params martini.Params) string {  
  //….  
}  
func updateItem(w http.ResponseWriter, r *http.Request, params martini.Params) (int, string) {  
  //…..  
}   
func addItem(w http.ResponseWriter, r *http.Request) (int, string) {  
 //….  
} 



    * 範例與實作的部分：



      * 我在練習的時候主要是利用[類似範例](http://0value.com/build-a-restful-API-with-Martini)裡面的in-memory database 還有 martini 來完成REST．此外並沒有用到JSON的格式，主要是因為之後打算拿來練習其他的資料庫mongoDB．


      * 實作的時候，我發現問題比較多的其實不外乎就是function的parameter 之外，再來就是client端要如何下指令去打成 GET/PUT/POST/DELETE 的指令，主要用的是CURL這裡簡單的列一下:



        * GET:  curl -i  "https://localhost:8001/albums”


        * POST:  curl -i POST --data "band=Carcass&title=Heartwork&year=1994" "https://localhost:8001/albums.xml”


        * PUT:  curl -i  PUT --data "band=Carcass&title=Heartwork&year=1993" "https://localhost:8001/albums/4”


        * DELETE: curl -i DELETE "https://localhost:8001/albums/1"




    * 參考:



      * [http://0value.com/build-a-restful-API-with-Martini](http://0value.com/build-a-restful-API-with-Martini)


      * [https://github.com/go-martini/martini](https://github.com/go-martini/martini)




