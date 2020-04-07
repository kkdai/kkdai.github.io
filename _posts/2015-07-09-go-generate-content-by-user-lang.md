---
layout: post
layout: post
title: "[Golang]如何處理多國使用者的網頁輸出"
description: ""
category: 
- golang
tags: []

---

![image](http://lh4.ggpht.com/-Gr_pNx2emRs/UpCz3yB40bI/AAAAAAAAB4E/VOLcEp8OsSA/image_thumb4.png?imgmax=800)

## 前言:

這裏整理一下，如何透過Golang來處理多國使用者網頁顯示的問題． 

所謂面對多國使用者的時候，主要有一些問題:

- 如何根據不同國家顯示不同資料
- 如何根據不同國家顯示正確的時間資料
- 如何根據不同國家顯示以及儲存正確的字元集

我在這裡只把顯示的部分整理一下....


## 判斷使用者國家語系方法:

#### 透過使用者設定

這個是最簡單的方式，也就是使用者自己先設定好你所偏好的語言．類似Google一樣，然後所有的語言顯示就會依據你的顯示方式．

但是這個方式可能有以下的問題，某些服務可能不只有在內部使用者使用．有可能會邀請不同的使用者． 這時候要如何顯示正確的語言呢？  

接著來看第二個判斷方式.....


#### 瀏覽器回傳數值

每個瀏覽器都會提供`Accept-Language`這個Header參數可以提供網站來參考．它的細節[在此](http://www.w3.org/International/questions/qa-accept-lang-locales)．

以下列出我用Golang 抓出的三個我常用的瀏覽器結果，在繁體中文Mac OSX上面．

1. Chrome: [zh-TW,zh;q=0.8,en-US;q=0.6,en;q=0.4]
2. Safari: [zh-tw]
3. FireFox: [zh-TW,zh;q=0.8,en-US;q=0.5,en;q=0.3]

        // Parse accept-language in header to convert it as: tw, en, jp ...
        func ParseAcceptLang(acceptLang string) string {
        	// 1. Chrome: [zh-TW,zh;q=0.8,en-US;q=0.6,en;q=0.4]
        	// 2. Safari: [zh-tw]
        	// 3. FF: [zh-TW,zh;q=0.8,en-US;q=0.5,en;q=0.3]
        	//
        	// Ret: zh or en ...
        	tarStrings := strings.Split(acceptLang, ";")
        	if len(strings.Split(tarStrings[0], ",")) > 1 {
        		return strings.Split(tarStrings[0], ",")[1]
        	}
        	return strings.Split(tarStrings[0], "-")[0]
        }

以上程式碼可以根據目前的Header的`Accept-Language` 內容轉換成 tw, en或是jp． 

##  針對不同國家語系，顯示不同內容

#### 使用javascript 顯示網頁部分

透過之前的`ParseAcceptLang`搭配著`http.Request`裡面的Header你就可以取出資料並且開始判斷與處理．這裏我使用的是[martini](https://github.com/go-martini/martini)跟 [martini-render](https://github.com/martini-contrib/render)．

        func ShowDifferentResultByLang(r render.Render, res *http.Request) {
        	var acceptLang string
        	for _, v := range res.Header["Accept-Language"] {
        		acceptLang = v
        		break
        	}
        
        	outPutData.Lang := OutPutStruct{}
            //取得瀏覽器Accept-Lang設定
            outPutData.Lang = ParseAcceptLang(acceptLang)
            r.HTML(200, "SOME_TMPL", outPutData)
        }
        
        ....
        
        //Web Template
        <script type="text/javascript">
            var Language = "\{\{ .Lang \}\}";
            DoSomethingWith(Language)
        </script>
 

這裡面的方法，就是要透過瀏覽器的回傳數值．透過JavaScript 來做相關的處理．  在JS裡面的細節我就不詳談了，這裡有一個詳細[方法](http://stackoverflow.com/questions/1043339/javascript-for-detecting-browser-language-preference)可以讓大家參考．

#### 顯示不同的圖片

除了在網頁上面透過Java Script來顯示不同的內容之外，其實就算要顯示圖片也是可以透過不同的瀏覽器來給予不同的語言．

方法其實很簡單，就是必須要多開一個接口來幫忙處理圖形的處理部分． 這裏主要會用到的是 `http.ResponseWriter`的方式．


        func ShowLocaleImageByBrowser(params martini.Params, r render.Render, db *DB_Medium, res http.ResponseWriter, req *http.Request) {
            //取得瀏覽器Accept-Lang設定
        	var acceptLang string
        	for _, v := range res.Header["Accept-Language"] {
        		acceptLang = v
        		break
        	}

            //轉換成可了解文字 en. tw. jp ...
            Lang := ParseAcceptLang(acceptLang)
            
            //根據不同語言讀取不同圖形
            imageAddr := GetImageAddrByLang(Lang)
            
            //直接將圖片寫回http.responseWriter
        	res.Header().Set("Content-Type", "image/jpeg")
        	err := jpeg.Encode(res, GetImage(imageAddr), &jpeg.Options{100})
        	if err != nil {
        		res.WriteHeader(500)
        	} else {
        		res.WriteHeader(200)
        	}
        }

	
## 心得:

這一篇把相關的部分做一個簡單的整理，也希望能找到更有效的方式來顯示正確的資訊給使用者．

## 參考鏈結:

- [stackoverflow: 如何使用JS來偵測瀏覽器的語言設定](http://stackoverflow.com/questions/1043339/javascript-for-detecting-browser-language-preference)
- [stackoverflow: 在Golang中回傳一張圖片](http://stackoverflow.com/questions/1043339/javascript-for-detecting-browser-language-preference)
