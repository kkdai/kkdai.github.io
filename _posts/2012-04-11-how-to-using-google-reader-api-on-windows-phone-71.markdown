---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-11 10:57:45+00:00
slug: how-to-using-google-reader-api-on-windows-phone-71
title: How to using google reader API on Windows Phone 7.1
wordpress_id: 1155
categories:
- NET程式設計
- VC++程式設計
- 學習文件
---

[Google data](http://code.google.com/intl/zh-TW/apis/gdata/) is good API tool for C# or other language to connect with Google related service, however here is no official [Google Reader](http://www.google.com/reader/) API on document. and [Google data](http://code.google.com/intl/zh-TW/apis/gdata/) don’t support for Windows phone (only for Windows mobile for now).

 

However it could be found on web for some of unofficial document for [Google Reader](http://www.google.com/reader/) API as follows:

 

  
  * [Unofficial Google Reader API](http://undoc.in/googlereader.html)
   
  * [R2 google reader on Google code](http://code.google.com/p/r2-release/wiki/GoogleReaderApi)
   
  * [pyfeed on Google code](http://code.google.com/p/pyrfeed/wiki/GoogleReaderAPI)
 

But however to come out detail implement for Windows Phone, it quit need time to work detail. Allow me try to list some major problems of this.

 

  
  * Most important of using HttpWebRequest is the error repose 
 

<blockquote>  
> 
> The remote server returned an error: NotFound
> 
> </blockquote>

 

It is most common error code your will find during you try to login or get any feedback on Window Phone. It could be either you missing your GET parameter or you missing your headers.

 

Here is some sample code related Login [Google Reader](http://www.google.com/reader/) and get list from this login. Some more detail communication code about HttpWebRequest, please check [http://msdn.microsoft.com/zh-tw/library/system.net.webrequest.begingetrequeststream(v=vs.80).aspx](http://msdn.microsoft.com/zh-tw/library/system.net.webrequest.begingetrequeststream(v=vs.80).aspx)

 

Something need to note before enter detail code.(check red part)

 

  
  * Need to authentication to get Auth_ID and SID for all API access. 
   
  * Don’t miss your “Headers” when you trying to get API.
   
  * Http method need follow as document. 
 

 
    
     public void LoginGoogleReader()
            {
                string email = "YOUR_EMAIL";
                string passwd = "YOUR_PASSWORD";
                string auth_params = string.Format("https://www.google.com/accounts/ClientLogin?accountType=HOSTED_OR_GOOGLE&Email=" + email + "&Passwd=" + passwd + "&service=reader&source=J-MyReader-1.0");
                HttpWebRequest httpRequest = (HttpWebRequest)WebRequest.Create(auth_params);
    <font color="#ff0000">            httpRequest.Method = "POST";</font>
                RequestState myRequestState = new RequestState();
                myRequestState.request = httpRequest;
                IAsyncResult result =
                    (IAsyncResult)httpRequest.BeginGetResponse(new AsyncCallback(AuthRespCallback), myRequestState);
            }
    
            public void GetGoogleReaderPreferenceList()
            {
                string auth_params = string.Format("https://www.google.com/reader/api/0/preference/list?<font color="#ff0000">client=scroll&ck=1293954081436</font>");
    
    <font color="#ff0000">            HttpWebRequest httpRequest = (HttpWebRequest)WebRequest.Create(auth_params);<br></br>            httpRequest.Method = "GET";
                httpRequest.Headers["Authorization"] = "GoogleLogin auth=" + UserAuth;
                httpRequest.Headers["Cookie"] = "SID=" + UserSid;
    </font>
                RequestState myRequestState = new RequestState();
                myRequestState.request = httpRequest;
    
                IAsyncResult result =
                    (IAsyncResult)httpRequest.BeginGetResponse(new AsyncCallback(AuthRespCallback), myRequestState);
            }
