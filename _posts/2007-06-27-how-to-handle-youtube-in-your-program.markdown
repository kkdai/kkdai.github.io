---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-06-27 14:07:59+00:00
slug: how-to-handle-youtube-in-your-program
title: How to handle YouTube in your program
wordpress_id: 687
categories:
- NET程式設計
---

[![Home](http://www.youtube.com/img/pic_youtubelogo_123x63.gif)](http://www.youtube.com/)

****

**Summarized:  
**[YouTube](http://www.youtube.com/) the world largest bideo braofcast company. People usually watch video under the web site or use the mashup to integrate into theirown blog. This article describes the way to deal with [YouTube](http://www.youtube.com/)  via C++ coding. I will show you how to handle [YouTube](http://www.youtube.com/)  API using Web Browser or using C++ to write a simple sample to demo how to get response from [YouTube](http://www.youtube.com/). Also I also tell you help to write a simple program to download [YouTube](http://www.youtube.com/) video via HTTP connection client.

**YouTube APIs  
**[YouTube](http://www.youtube.com/) already provide lots of APIs which could help us to query video detail, get list by tag, list popular... All you have to do is open a url which like

<blockquote>[http://www.youtube.com/api2_rest?method=youtube.videos.list_by_tag&dev_id=XXXX_ID&tag=TakeThat](http://www.youtube.com/api2_rest?method=youtube.videos.list_by_tag&dev_id=XXXX_ID&tag=TakeThat)
> 
> </blockquote>

As you see the URL it separate into three part:

  1. "method=youtube.videos.list_by_tag": it   
is API name, whole API list is [here](http://www.youtube.com/dev_docs).   

  2. "dev_id=XXXX_ID": Every API request need a developer ID which you can request [here](http://www.youtube.com/my_profile_dev).  

  3. "&tag=TakeThat": This API could help to figure out the list of specific tag. Oh my favorite band is "Take That" ^_^.

After you send the Http request via your browser, you would get the result XML as follow:

[![Y_XML](http://farm2.static.flickr.com/1190/636578472_3a4ee1f4d6_m.jpg)](http://www.flickr.com/photos/evanlin/636578472/)

Yes, a XML based response which you could full handle it in any kinds of program you like.

**Download YouTube:  
**Pick up any video from [YouTube](http://www.youtube.com/) which like: [http://youtube.com/watch?v=abK9WNFbKus&eurl=%2Findex](http://youtube.com/watch?v=abK9WNFbKus&eurl=%2Findex). How to download it via Http connection? First I want to introduce a web site which can help you to transform your [YouTube](http://www.youtube.com/) URL to a download-able address. [男丁格爾's 脫殼玩 - FLV Video Parser Web 版](http://209.8.117.203/~abgne/flv/action.php) is a one of this web site. 

Because any playback from [YouTube](http://www.youtube.com/) need request a SK(I don't know what is stand for). How to get the SK? You can paste a URL like [http://youtube.com/watch?v=abK9WNFbKus&eurl=%2Findex](http://youtube.com/watch?v=abK9WNFbKus&eurl=%2Findex), and you open the source code of this page (Use IE to right click and "View Source"). Search a strinbg as follow:

<blockquote> function writeMoviePlayer(player_div, force)  
 {  
  var v = "7";  
  if (force)  
   v = "0";
> 
>   var fo = new SWFObject("/player2.swf?hl=en&BASE_YT_URL=http://youtube.com/&video_id=1vjLC_gcftE&l=121&t=OEgsToPDskIwzbAyRDMe7MOVOjVzt-Qf&soff=1&sk=CxfMnY-qWhLIp4nOKIFWOgU", "movie_player", "450", "370", v, "#FFFFFF");
> 
>    fo.addVariable("playnext", 0);  
  fo.addVariable("hl", getQueryParamValue("hl"));  
  fo.addParam("allowFullscreen", "true");  
    fo.addVariable("sourceid", "yw");  
    fo.addVariable("sdetail", "m%3Auser%2Crv%3AabK9WNFbKus");  
  player_written = fo.write(player_div);  
 }
> 
> </blockquote>

Use the "RED" string and connect with [http://www.youtube.com/get_video?](http://www.youtube.com/get_video?) it should be [http://www.youtube.com/get_video?video_id=1vjLC_gcftE&l=121&t=OEgsToPDskIwzbAyRDMe7MOVOjVzt-Qf&soff=1&sk=CxfMnY-qWhLIp4nOKIFWOgU](http://www.youtube.com/get_video?video_id=1vjLC_gcftE&l=121&t=OEgsToPDskIwzbAyRDMe7MOVOjVzt-Qf&soff=1&sk=CxfMnY-qWhLIp4nOKIFWOgU) it could download the video from [YouTube](http://www.youtube.com/) directly. (The URL may invalid after a period time, please request it again.)

**Sample Program:  
**Any handle of [YouTube](http://www.youtube.com/) could via Http connection clien which IE/FireFox... [CHttpClient](http://www.codeproject.com/library/lyoulhttpclient.asp) is a very useful simple http client program which write by C++.  

![](http://www.codeproject.com/library/lyoulhttpclient/ryeolhttpclient.jpg)

It has a clear GUI and useful connection API to get/post and request to Web Site via HTTP. I modify this application and change it to deal with [YouTube](http://www.youtube.com/).

Please download in   
[http://www.badongo.com/file/3563196](http://www.badongo.com/file/3563196)

Any comment is welcome.
