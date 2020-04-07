---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-28 05:12:44+00:00
slug: cwindows-phonejson%e5%a6%82%e4%bd%95%e7%94%a8c%e5%ae%8c%e6%88%90%e4%b8%80%e5%80%8b%e7%b0%a1%e5%96%ae%e7%9a%84google-reader-app%e4%b8%8a
title: '[C#][Windows Phone][JSON]如何用C#完成一個簡單的Google Reader APp(上)'
wordpress_id: 1160
categories:
- NET程式設計
- 學習文件
---

這個禮拜初，最後總算弄好標記閱讀(Mark Read)後，Google Reader App總算也完成了。不過由於接下來可能會把一些時間拿去研究iOS。最後決定把這部份的研究做一個總整理。這一篇文章會包括以下的技術:

 

  
  * Google API 在Windows Phone 7.1上面的相關使用 
   
  * 對於Google Reader API的JSON的相關使用 
   
  * 一些簡單的Windows Phone原始碼(source code)的討論 
 

_關於RSS Reader App:_

 

首先我想在這裡先稍微整理一下，一個簡單的RSS Reader的Windows Phone APP是相當簡單的。其實只要會簡單的Webclient加上去使用內建的資料格式就可以完成，這裡[微軟也提供完整的原始碼與介紹](http://msdn.microsoft.com/en-us/library/hh487167(v=vs.92).aspx)。本篇介紹的部分主要都是專注在Google Reader的部分。 

 

_Google Reader API:_

 

由於我是在Windows Phone上面做開發(Windows Phone 7.1)所以無法直接使用[Google Data](https://developers.google.com/gdata/?hl=zh-TW)上面的相關API，在這裡也要特別解釋，如果你只是WPF或是一般Windows程式~ 你可以直接使用[Google Data](https://developers.google.com/gdata/?hl=zh-TW)而不需要一步步的完成Google Reader API的部分，在這裡把之前文章列過的Google Reader API整理一次:

 

  
  * [Unofficial Google Reader API](http://undoc.in/googlereader.html)
   
  * [R2 google reader on Google code](http://code.google.com/p/r2-release/wiki/GoogleReaderApi)
   
  * [pyfeed on Google code](http://code.google.com/p/pyrfeed/wiki/GoogleReaderAPI)
 

_一個簡單Google Reader APP的流程:_

 

接下來我會用碼個Google Reader 會使用到的流程來介紹該如何去撰寫，首先一個Google Reader APP會有以細的流程:

 

  
  1. 登入，也就是登入你的Google Reader 帳號 
   
  2. 取得登入者的相關資訊 
   
  3. 瀏覽全部尚未閱讀的列表 
   
  4. 打開某個標籤(Label)內去看裡面還沒閱讀的文章 
   
  5. 打開該文章網站並且將其標記已經閱讀(mark as read) 
 

接下來我會針對以下的部分，開始整理一些原始碼:

 

**1. 登入，也就是登入你的Google Reader 帳號**       
在這裡會使用到基本的Google 登入部分，你可以直接在你的瀏覽器上頭打入以下的鏈結。

 

<blockquote>  
> 
> [https://www.google.com/reader/api/0/token?ck=212121212&client=scroll](https://www.google.com/reader/api/0/token?ck=212121212&client=scroll)
> 
> </blockquote>

 

稍微解釋一下參數:

 

<blockquote>  
> 
> ck: 就是時間標記(timestamp)，你可以用DateTime.Now.Ticks.ToString()拿到
> 
>    
> 
> client:先填入scroll的方式
> 
> </blockquote>

 

如果你已經登入過的話，一樣會有一樣的結果      


 
    
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






    
但是你也需要去把登入回來的SID與AuthID去parse出來，以下提供相關方法:

    





    
    private void ParseLogin(string responseString)
            {
                string[] arrs = responseString.Split('n');
                foreach (string arr in arrs)
                {
                    //Such as follows, but we only need Auth and SID
                    //SID=DQAAALcAAACH5nzd2xVxumGUJGjoTT9DblBfAuI1Sj-6Evtu_91q8Fkbis_S1qv66-vatdUe9HMqqELN3AijrD-CiQyzqJtr_XgTtJIDnfDeVmoGjtsSpkyIB4sES5slrOkTvRJd67dG0tlwTih8btejcPqAVYCJMD6f-a-x3PHa2JQ-Ehsh-rtjNzW6y7Riqosyawffvg4DPbtvUJFsIsum8d32EQ_XuxmUM7NFuhwW5OG8aw3lLO_jO0fLaznl7n5DfCC61KSAn
                    //LSID=DQAAALkAAAAAeAUiJHtt2vL41E3bfMAJwE_waG3qEtAgEsv-Il0xXznDCdm-Z_jQZ4Log9DbGqTMd1-t08udWBJWeQ9VDG0uOC4H5nB0zJ_WGv1E17v3EVeveemKvpu9eN2YBkQlr6hMtZlZWyAb5w0uwAx6kPdXnnuuYC4o0RHv2em0CrOAFzpNYZvLOhuB_veFZ9bsnPy6GP0_HHQGe2o3dJsoJK_DKyq85QteslDzcQySldfwNGUy46Q4HLKhZZPDrjnO_eUn
                    //Auth=DQAAALgAAAAAeAUiJHtt2vL41E3bfMAJta7kSZRtYIzGfm8uJU_jVFjmIFbYYL9WaLS7Xj3xqdwLOrzrBipqL8ItZks4Hf71NY2yTyZnAIG5ysrlA9kCcoZGDDqo3ib9avvgC4pPwXB2uQ3rBYt0gqYs28DkEX6fDD4S3j_NwBESynhOUhcTKqhN3pYX1VfH6uU4285yV7O3w7NKfF8kkTOEFl5toztOwnA4JWnbC5Rjb_gMXKmKnzayTMevgO_XfGWqqNa8x5Mn"	string
                    string[] tmp = arr.Split('=');
                    if (tmp[0] == "Auth")
                    {
                        UserAuth = tmp[1];
                    }
                    else if (tmp[0] == "SID")
                    {
                        UserSid = tmp[1];
                    }
                }
            }





記得把UserAuth與UserSid要儲存下來，之後每一個動作都需要。









**2. 取得登入者的相關資訊**





登入以後我們需要去取得使用者的ID，這樣主要也是給我們去找尋標籤(Label)或是做一些使用者的相關功能來使用。





如同之前一樣，你也可以在你的瀏覽器上面打入以下的鏈結來看結果:





<blockquote>
  
> 
> [https://www.google.com/reader/api/0/user-info?format=json&ck=121212122&client=scroll](https://www.google.com/reader/api/0/user-info?format=json&ck=121212122&client=scroll)
> 
> 
</blockquote>





這裡多了一個參數，也就是JSON的輸出格式





<blockquote>
  
> 
> format: 如果不是寫joson會傳回XML資料
> 
> 
</blockquote>





你打出來的使用者資訊應該會像以下的資料一樣:




    
    {
    "userId":"XXX",
    "userName":"XXX",
    "userProfileId":"XXX",
    "userEmail":"XXX@gmail.com",
    "isBloggerUser":true,
    "signupTimeSec":1188451343,
    "publicUserName":"XXX",
    "isMultiLoginEnabled":true
    }









而我們需要的也只有userProfileId，接下來先給大家如何去抓取ID的程式碼:




    
    public void GetUserInformation()
            {
                string auth_params = string.Format("https://www.google.com/reader/api/0/user-info?format=joson&ck=" + DateTime.Now.Ticks.ToString() + "&client=scroll");
    
                HttpWebRequest httpRequest = (HttpWebRequest)WebRequest.Create(auth_params);
                httpRequest.Method = "GET";
                httpRequest.Headers["Authorization"] = "GoogleLogin auth=" + UserAuth;
                httpRequest.Headers["Cookie"] = "SID=" + UserSid;
                httpRequest.BeginGetResponse(new AsyncCallback(ResponseCallbackInner), httpRequest);
            }









在這裡會使用到之前抓的兩個參數UserAuth以及UserSid記得要填到Header之內不然都會得到error 400。





如果正確的拿到資料的話，我也提供利用JSON來抓取資料的方式。




    
    public class ServerUserInfo //JSON
            {
                public string userId { get; set; }
                public string userName { get; set; }
                public string userProfileId { get; set; }
                public string userEmail { get; set; }
                public string isBloggerUser { get; set; }
                public string signupTimeSec { get; set; }
                public string publicUserName { get; set; }
                public string isMultiLoginEnabled { get; set; }
            }
    
    
    private void ParseUserInfo(string responseString)
            {
                UserInfoClass = JsonConvert.DeserializeObject<ServerUserInfo>(responseString);
                staUserID = UserInfoClass.userId;
            }









看到了嗎?這就是JSON神奇的地方，你只要一行程式碼就可以去把所有資料提到你的資料UserInfoClass(是class ServerUserInfo 產生的)中。





先寫到這個部分，接下來就是瀏覽標籤(Label)與找到文章內容的部分…
