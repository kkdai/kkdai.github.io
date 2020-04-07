---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-29 07:17:20+00:00
slug: cwindows-phonejson%e5%a6%82%e4%bd%95%e7%94%a8c%e5%ae%8c%e6%88%90%e4%b8%80%e5%80%8b%e7%b0%a1%e5%96%ae%e7%9a%84google-reader-app%e4%b8%8b
title: '[C#][Windows Phone][JSON]如何用C#完成一個簡單的Google Reader APP(下)'
wordpress_id: 1161
categories:
- NET程式設計
- 學習文件
---

****

 

**3. 瀏覽全部尚未閱讀的列表**

 

接上篇文章，接下來會開始講解如何去獲得使用者所有標籤(Label)的文章。

 

一開始首先一樣是先提供這裡會用到的相關Google Reader API的講解

 

<blockquote>  
> 
> [https://www.google.com/reader/api/0/unread-count?allcomments=true&output=json&ck=12121&client=scroll](https://www.google.com/reader/api/0/unread-count?allcomments=true&output=json&ck=12121&client=scroll)
> 
> </blockquote>

 

你就可以在瀏覽器上面獲得你要的資訊。你如果有登入你可能會獲得像是以下的一些資料。

 

接下來是取得資料的相關原始碼:

 
    
    public void GetGoogleReaderUnReadCount()
            {
                string auth_params = string.Format("https://www.google.com/reader/api/0/unread-count?allcomments=true&output=json&ck=" + DateTime.Now.Ticks.ToString() + "&client=scroll");
    
                HttpWebRequest httpRequest = (HttpWebRequest)WebRequest.Create(auth_params);
                httpRequest.Method = "GET";
                httpRequest.Headers["Authorization"] = "GoogleLogin auth=" + UserAuth;
                httpRequest.Headers["Cookie"] = "SID=" + UserSid;
                httpRequest.BeginGetResponse(new AsyncCallback(ResponseCallbackInner), httpRequest);
            }









抓回資料後，就開始要去parse這些資料，這邊會用到大量的JSON相關技巧，我也盡量試著解釋清楚一點給大家知道。首先，會拿到以下的資料，以下是以我的資訊當作範例～跟你拿到的可能會有點差異。




    
    {
    "max":1000,
    "unreadcounts":[
    	{
    		"id":"user/-/label/教學網站",
    		"count":9,
    		"newestItemTimestampUsec":"1335595557150290"
    	},
    	{
    		"id":"user/-/label/Funny",
    		"count":83,
    		"newestItemTimestampUsec":"1335621983685977"
    	},
    	{
    		"id":"user/-/label/專業評論",
    		"count":38,
    		"newestItemTimestampUsec":"1335617137314302"
    	}
    }





這些資料就是JSON的資料，關於JSON是甚麼與JSON.NET該怎麼使用的部分，網路上有許多相關的程式碼，我這裡就不詳述了。





根據上面資料排序的結果，我們需要去抓unreadcounts下面的子結點，而他的資料格式如下:




    
    public class ServerUnreadResult //JSON
            {
                public string id { get; set; }
                public string count { get; set; }
                public string newestItemTimestampUsec { get; set; }
            }





所以在我們程式碼中，就是可以依照以下的安排:




    
    private void ParseUnreadList(string responseString)
            {
                //JSON Parse
                JObject googleSearch = JObject.Parse(responseString);
    
                // get JSON result objects into a list
                IList<JToken> results = googleSearch["unreadcounts"].Children().ToList();
    
                // serialize JSON results into .NET objects
                IList<ServerUnreadResult> searchResults = new List<ServerUnreadResult>();
                foreach (JToken result in results)
                {
                    ServerUnreadResult searchResult = JsonConvert.DeserializeObject<ServerUnreadResult>(result.ToString());
    
                    //Parse the label name
                    // ex: user/06771113693638414260/label/Win8
                    string LabelID = searchResult.id;
                    string[] arrs = LabelID.Split('/');
                    if (!arrs[0].Contains("user"))
                        continue;
                    LabelID = arrs[3];
    	// Add to Your arrary...
    	.......
                }
            }





開始講解這段的程式碼，主要就是第7行的部分可以直接選取到unreadcounts子結點下面所有的內容。然後再針對所有子結點開始DeserializeObject，這樣就可以把所有資料填入到ServerUnreadResult 所產生的類別資料中。這裡主要需要的是標籤名稱與尚未讀取的文章數目。









**4. 打開某個標籤(Label)內去看裡面還沒閱讀的文章**





當你知道某個標籤內有多少文章，使用者就應該可以點選該標籤去看某個標籤內的所有文章。所以我們需要開始去處理如何查詢某個標籤內所有尚未閱讀的文章。





接下來的部分有點小複雜的是，因為傳回來的JSON資料會相當的多，所以可能需要一些工具去幫助你了解JSON資料。個人推薦[http://json.parser.online.fr/](http://json.parser.online.fr/)這個網站，他可以把你JSON回傳得資料展開。可以幫助你看清楚資料裡面的階層關係。 





跟之前的介紹一樣，一開始先提供相關鏈結:





<blockquote>
  
> 
> [http://www.google.com/reader/api/0/stream/contents/user/"](http://www.google.com/reader/api/0/stream/contents/user/") + UserInfoClass.userId + "/label/"+sLabel+"?n=15&ck=" + DateTime.Now.Ticks.ToString() + "&client=scroll&format=json&xt=user/" + UserInfoClass.userId + "/state/com.google/read
> 
> 
</blockquote>





跟之前的指令一樣你只要把UserInfoClass.userId換成你的ID還有把DateTime.Now.Ticks.ToString() 換成任何一個隨意的值。你就可以在瀏覽器上面獲得你要的資訊。這裡有一個參數要特別講解。





<blockquote>
  
> 
> &xt=user/-/state/com.google/read
> 
> 
</blockquote>





這個參數就是幫你把你讀過的部分去除掉，也就是挑選你"尚未閱讀的部分"。





此外另外一個參數n=15 就是顯是你要幾筆資料，你可以將前一個動作所取出來所有尚未讀取的文章放在這裡，預設值是給20。





接下來就是提供給大家相關的程式碼的部分:




    
    public void GetATOMbyLabel(string sLabel)
            {
                string auth_params = string.Format("http://www.google.com/reader/api/0/stream/contents/user/" + UserInfoClass.userId + "/label/"+sLabel+"?n=15&ck=" + DateTime.Now.Ticks.ToString() + "&client=scroll&format=json&xt=user/" + UserInfoClass.userId + "/state/com.google/read");
    
                HttpWebRequest httpRequest = (HttpWebRequest)WebRequest.Create(auth_params);
                httpRequest.Method = "GET";
                httpRequest.Headers["Authorization"] = "GoogleLogin auth=" + UserAuth;
                httpRequest.Headers["Cookie"] = "SID=" + UserSid;
                httpRequest.BeginGetResponse(new AsyncCallback(ResponseCallback), httpRequest);
            }









這邊沒有太多程式碼的部分需要說明。





接下來就要處理回傳的JSON資料，這僅僅是一筆的資料如下，由於資料有點大量，我盡量刪除不需要的部分:




    
    {
    "direction":"ltr",
    "id":"user/-/label/Apple",
    "title":"在 Google 閱讀器中取得 Evan Lin 的「Apple」",
    "continuation":"COGG3sTqta8C",
    "self":[
    {
    "href":"http://www.google.com/reader/api/0/stream/contents/user/-/label/Apple?nu003d15u0026cku003d2323232323u0026clientu003dscrollu0026formatu003djson"
    }
    ],
    "author":"Evan Lin",
    "updated":1334555570,
    "items":[
    {
    "id":"tag:google.com,2005:reader/item/f1c6c6aed52d2682",
    "title":"【限時免費】Moco Cam：復古底片當道，LOMO 鏡頭隨你拍，超過20種濾鏡效果，原價0.99，限時免費下載中",
    "alternate":[
    {
    "href":"http://funiphone.pixnet.net/blog/post/37297964",
    "type":"text/html"
    }
    ],
    "content":{
    "direction":"ltr",
    "content":"u003cpu003eu003cimg srcu003d"http://pic.pimg.tw/funiphone/1334549031-3221087701_q.png" altu003d"" widthu003d"150"u003eu003cimg srcu003d"http://pic.pimg.tw/funiphone/1334549497-4194937028_n.png" altu003d"" widthu003d"450"u003eu003c/pu003enu003cpu003e u003c/pu003enu003cpu003e u003c/pu003enu003cpu003eu003ca hrefu003d"http://itunes.apple.com/app/moco-cam-camera-lens-effects/id371271476?mtu003d8"u003eu003cimg styleu003d"display:block;margin-left:auto;margin-right:auto" srcu003d"http://pic.pimg.tw/funiphone/1310632558-92007661eba0bc5e489df3d66cfff9b9.jpg?vu003d1310632559" altu003d"按此下載.jpg"u003eu003c/au003eu003c/pu003enu003cpu003eu003cimg srcu003d"http://a1.mzstatic.com/us/r1000/059/Purple/b6/38/6b/mzl.fgzqmjnx.320x480-75.jpg" altu003d"iPhone Screenshot 1" widthu003d"280"u003e u003cimg srcu003d"http://a5.mzstatic.com/us/r1000/038/Purple/ff/8a/7c/mzl.fzmsyokn.320x480-75.jpg" altu003d"iPhone Screenshot 3" widthu003d"280"u003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:14pt"u003eu003cspanu003e【目前售價：u003cspan styleu003d"color:#ff0000"u003e限時免費u003c/spanu003eu003c/spanu003eu003cspanu003e】u003c/spanu003eu003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:14pt"u003e【檔案大小：5.0 MB】u003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003eMoco Cam (Camera Lens Effects) 限時免費的好用鏡頭濾鏡軟體，這個App介面十分簡單，但支援超過20幾種濾鏡。原價0.99美金，限時免費中，喜歡用iPhone、iPad等iOS裝置照相拍照的話，可以趁限免時下載此程式App。u003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003e（下為 iTunes 上的玩家評價）u003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003eu003cimg titleu003d"89" srcu003d"http://pic.pimg.tw/funiphone/1334549972-1765202292.png" altu003d"89" borderu003d"0"u003e u003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large;color:#0000ff"u003eFun iPhone(APP玩家)評分：★★★u003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003eu003cbru003eu003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003e此相機修圖程式的首頁介面非常簡單，其實也有點陽春。按下方咖啡色方塊可進入程式。u003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003eu003cimg srcu003d"http://a5.mzstatic.com/us/r1000/030/Purple/29/f7/f2/mzl.xxifelxh.320x480-75.jpg" altu003d"" widthu003d"320"u003eu003c/spanu003eu003c/pu003enu003cpu003eu003cspan styleu003d"font-size:large"u003e要選擇 take a photo 相機照相或直接從手機相簿中選擇想加入濾鏡的圖片u003c/spanu003eu003c/pu003e u003cdivu003eu003ca hrefu003d"http://funiphone.pixnet.net/blog/post/37297964"u003e(繼續閱讀...)u003c/au003eu003cimg srcu003d"http://pixanalytics.com/pa.gif?tu003dfront_blog_feedu0026amp;document.URLu003dhttp://funiphone.pixnet.net/blog/post/37297964"u003eu003c/divu003e"
    },
    "origin":{
    "streamId":"feed/http://funiphone.pixnet.net/blog/feed/rss",
    "title":"Fun iPhone 愛瘋玩家 (APP玩家):: 痞客邦 PIXNET ::",
    "htmlUrl":"http://funiphone.pixnet.net/blog"
    }
    }
    }





這邊資料有點多，我盡量去分開講。這裡大家會用到一些資料內容主要有以下的資料。






  
  * ["id”]: 這代表了這篇文章在google reader中的唯一辨別碼，很多地方用的到。 


  
  * ["title”]: 這個是這篇文章的標題 


  
  * ["alternate"]["href"]: 這是這篇文章原來的網址，可以用來展開到瀏覽器用。 


  
  * ["origin"]["streamId"]: 這個是這個feed的ID，之後在標記閱讀會用到 





提供程式碼之前，一樣的我們需要把資料先宣告成類別如下:




    
    public class ServerLabelATOMResult //JSON
            {
                public string id { get; set; }
                public string title { get; set; }
            }
    
            public class ServerLabelATOMResult2 //JSON
            {
                public string href { get; set; }
                public string type { get; set; }
                //public string content { get; set; }
            }





接下來就提供程式碼的部分:




    
    private void ParseLabelATOM(string responseString)
            {
                //JSON Parse
                JObject googleSearch = JObject.Parse(responseString);
    
                // get JSON result objects into a list
                IList<JToken> results = googleSearch["items"].Children().ToList();
    
                // serialize JSON results into .NET objects
                IList<ServerLabelATOMResult> searchResults = new List<ServerLabelATOMResult>();
                foreach (JToken result in results)
                {
                    ServerLabelATOMResult searchResult = JsonConvert.DeserializeObject<ServerLabelATOMResult>(result.ToString());
                    string newsID = searchResult.id;
                    string feedID = result["origin"]["streamId"].ToString();
                    string feedTitle = result["origin"]["title"].ToString();
    
                    ///// Get the link of this news.
                    string sLink = "";
                    IList<JToken> results2 = result["alternate"].Children().ToList();
                    foreach (JToken result2s in results2)
                        sLink = result2s["href"].ToString();
    	//Add to your data set here
                }
            }





這裡要特別講解一下，由於JSON裡面的LIST 主要是處理多個資料集(LIST)的方式，就如同對應到JSON的





<blockquote>
  
> 
> ":["
> 
> 
</blockquote>





所以在這裡的["alternate"]["href"]必須當作是多個資料，雖然Google本身也只有包了一個資料集而已，也就是說雖然Google裡面只有"href"與"http"共一個資料集(LIST)但是我們還是得這樣處理他，不然拿不到資料。這個部分也弄了很久，在此分享給大家。





**5. 打開該文章網站並且將其標記已經閱讀(mark as read)**





這邊的部分就請參照我之前的文章，在這裡就不再詳述了。
