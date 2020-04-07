---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-23 18:49:51+00:00
slug: cwindows-phone%e5%a6%82%e4%bd%95%e4%bd%bf%e7%94%a8google-reader-api%e6%a8%99%e8%a8%98%e5%b7%b2%e7%b6%93%e9%96%b1%e8%ae%80-mark-as-read
title: '[C#][Windows Phone]如何使用Google reader API標記已經閱讀 (mark as read)'
wordpress_id: 1158
categories:
- NET程式設計
- 學習文件
---

雖然網路上沒有完整使用C#與Windows Phone 7.1的範例，不過還是盡量用中文來寫這篇吧。其實網路上英文的資料應該也不少，不論是[pyrfeed這裡的Google API列表](http://code.google.com/p/pyrfeed/wiki/GoogleReaderAPI)，還是[Unofficial Google Reader API](http://undoc.in/googlereader.html)或是[R2 google reader on Google code](http://code.google.com/p/r2-release/wiki/GoogleReaderApi)。不過要完整的C#還有Windows Phone 7.1的sample code我也是想辦法K了半天這三份API文件然後不斷的測試，在這裡就列出一些片段的程式碼。

 
    
    public void MarkArticleAsRead(string newID)
    {
                string auth_params = string.Format("https://www.google.com/reader/api/0/edit-tag?client=scroll&format=joson&ck=" + DateTime.Now.Ticks.ToString());
    
                HttpWebRequest httpRequest = (HttpWebRequest)WebRequest.Create(auth_params);
                httpRequest.Method = "POST";
                httpRequest.ContentType = "application/x-www-form-urlencoded;charset=UTF-8";
                httpRequest.Headers["Authorization"] = "GoogleLogin auth=" + UserAuth;
                httpRequest.Headers["Cookie"] = "SID=" + UserSid;
                httpRequest.BeginGetRequestStream(new AsyncCallback(GetPostRequestStreamCallback), httpRequest);
            }    









不過這裡有一些需要注意的是SID與auth ID需要另外區擷取，這在我其他文章提到。不過接下來就是重點的部分。也就是post data該如何填、該放哪些資料。




    
     private static void GetPostRequestStreamCallback(IAsyncResult asynchronousResult)
            {
                HttpWebRequest request = (HttpWebRequest)asynchronousResult.AsyncState;
    
                // End the operation
                Stream postStream = request.EndGetRequestStream(asynchronousResult);
                string postData = "";
                postData += "&i=tag:google.com,2005:reader/item/fa42c976c848ecf4";
                postData += "&a=user/-/state/com.google/read";
                postData += "&s=feed/http://funiphone.pixnet.net/blog/feed/rss";
                // Note the token must get within 30 mins
                postData += "&T=//mUESPUMtDyZh6BaFXd-CqQ" + LoginToken;
    
                // Convert the string into a byte array.
                byte[] byteArray = Encoding.UTF8.GetBytes(postData);
    
                // Write to the request stream.
                postStream.Write(byteArray, 0, postData.Length);
                postStream.Close();
    
                // Start the asynchronous operation to get the response
                request.BeginGetResponse(new AsyncCallback(GetPostResponseCallback), request);
            }





在這裡也要注意的是Token需要再30分鐘之內取得~ 建議是呼叫之前馬上去取得token來使用。還有這裡的參數”a=”user 之後有user id 需要填，在這裡就不列出來。 &i=要填的是news 的id，還有&s=要填的是rss ID，你可以在另外一個參數”[http://www.google.com/reader/api/0/stream/contents/user](http://www.google.com/reader/api/0/stream/contents/user)” 去取得。最後是結果的部分:




    
    private static void GetPostResponseCallback(IAsyncResult asynchronousResult)
            {
                HttpWebRequest request = (HttpWebRequest)asynchronousResult.AsyncState;
    
                // End the operation
                HttpWebResponse response = (HttpWebResponse)request.EndGetResponse(asynchronousResult);
                Stream streamResponse = response.GetResponseStream();
                StreamReader streamRead = new StreamReader(streamResponse);
                string responseString = streamRead.ReadToEnd();
                // "responseString" should be "OK" if post succucess
    
                // Close the stream object
                streamResponse.Close();
                streamRead.Close();
                response.Close();
            }









這裡重要的是~ 如果這篇文章被mark as read你就會得到OK，但是如果之前已經mark as read你就會得到一個exception，這是需要注意的。





目前先寫到這裡~ 找時間把全部的流程跑一次~ 順便講解一下JSON的部分。
