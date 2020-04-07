---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-25 09:05:42+00:00
slug: cwindows-phone%e5%88%a9%e7%94%a8webclient%e5%8e%bb%e5%ae%8c%e6%88%90google-reader%e6%a8%99%e8%a8%98%e9%96%b1%e8%ae%80mark-as-read
title: '[C#][Windows Phone]利用WebCLient去完成Google Reader標記閱讀(Mark as read)'
wordpress_id: 1159
categories:
- NET程式設計
- 學習文件
---

上一篇文章有講解利用HttpWebRequest的方式來完成Google Reader中的標記閱讀，但是因為edit-tag本身就不屬於太消耗時間的動作。與其去用HttpWebRequest的非同步的方式，不如使用容易又好用的WebClient，所以我就把我原來程式碼做了一點修改。

 
    
     public void MarkArticleAsRead()
            {
                CurrentTransType = Transaction_Type.MARK_AS_READ;
                string auth_params = string.Format("https://www.google.com/reader/api/0/edit-tag?client=scroll&format=joson&ck=" + DateTime.Now.Ticks.ToString());
    
                string postData = "";
                postData += "&i=tag:google.com,2005:reader/item/fa42c976c848ecf4";
                postData += "&a=user/-/state/com.google/read";
                postData += "&s=feed/http://funiphone.pixnet.net/blog/feed/rss";
                // Note the token must get within 30 mins
                postData += "&T=//mUESPUMtDyZh6BaFXd-CqQ";
    
    
                WebClient wc = new WebClient();
                wc.Headers["Content-type"] = "application/x-www-form-urlencoded";
                wc.Headers["Authorization"] = "GoogleLogin auth=" + UserAuth;
                wc.Headers["Cookie"] = "SID=" + UserSid;
                try
                {
                    wc.UploadStringAsync(new Uri(auth_params), "POST", postData);
                }
                catch (WebException e)
                {
                    //handle error if any
                }
            }
    <font face="Georgia">幾個需要注意的地方如下:</font>






  
  * 
    
    
    <font face="Georgia">UserAuth與UserSid 需要先對Google API做login的部分</font>


  


  
  * 
    
    
    <font face="Georgia">Content-type是必須要先填成這樣，我試過uff8會失敗~</font>


  




    
    <font face="Georgia"></font>
