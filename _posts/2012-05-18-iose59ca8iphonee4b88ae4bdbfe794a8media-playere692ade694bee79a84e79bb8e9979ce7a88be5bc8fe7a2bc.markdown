---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-05-18 13:39:11+00:00
slug: ios%e5%9c%a8iphone%e4%b8%8a%e4%bd%bf%e7%94%a8media-player%e6%92%ad%e6%94%be%e7%9a%84%e7%9b%b8%e9%97%9c%e7%a8%8b%e5%bc%8f%e7%a2%bc
title: '[iOS]在iPHONE上使用MEDIA PLAYER播放的相關程式碼'
wordpress_id: 1164
categories:
- iOS的程式開發
---

在iOS要撥放影片其實有兩種播放器的source可以使用。
一個就是Media Player (MPMoviePlayerController)，另一個是AVPlayer (AV Foundation)。

這裡會介紹一些簡單使用Media Player的相關程式碼。
主要有幾個部分，第一個部份就是設定撥放來源，在這裡是使用網路上的檔案。

接下來的部分就是關於撥放得相關設定

    
    //強迫旋轉你的iDevice (這裡是橫放)
    [[UIApplication sharedApplication] setStatusBarOrientation:UIInterfaceOrientationLandscapeRight animated:YES];
    
     // 設定撥放器的外觀
     [WebPlayer setControlStyle:MPMovieControlStyleFullscreen];
            [WebPlayerr setFullscreen:YES];
    
     //設定影片比例的縮放、重複、控制列等參數
    
    //影片旋轉90度
    WebPlayer.view.transform = CGAffineTransformMakeRotation(1.5707964);
    WebPlayer.scalingMode = MPMovieScalingModeAspectFit;
    
    WebPlayer.repeatMode = MPMovieRepeatModeNone;
    
    //將影片加至主畫面上
    
    WebPlayer.view.frame = self.view.bounds;
    
    [self.view addSubview:WebPlayer.view];
    
    //開始播放
     [WebPlayer play];


這樣其實就可以撥放全螢幕的影片並且將手機橫放

如果播放完影片要可以回到原來的畫面，則要多接收一個Observer.

    
    // 註冊endPlay 當撥放完畢的時候，會收回傳的通知
    
     [[NSNotificationCenter defaultCenter] addObserver:self
                                                     selector:@selector(endPlay:)
                                                         name:MPMoviePlayerPlaybackDidFinishNotification
                                                       object:WebPlayer];


最後就是放endPlay的所有內容，裡面的重點在釋放撥放器與回到iDevice相關設定

    
    -(void)endPlay: (NSNotification*)notification
    {
    	//解除註冊回傳
    	[[NSNotificationCenter defaultCenter] removeObserver:self 	name:MPMoviePlayerPlaybackDidFinishNotification object:WebPlayer];
    
    	//將你的iDevice 轉回來
    	 [[UIApplication sharedApplication] 	setStatusBarOrientation:UIInterfaceOrientationPortrait animated:YES];
    
    	//停止播放
    	 [WebPlayer stop];
    	//Release control
    
    	[WebPlayer release];
    
    	//現在的view 移除~ 回到原本的view
    	[WebPlayer.view removeFromSuperview];
    
    }


參考文章:



	
  * [Furnace iOS 程式設計中文學習網站: 使用_MPMoviePlayerController_](http://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CGYQFjAA&url=http%3A%2F%2Ffurnacedigital.blogspot.com%2F2011%2F02%2Fmpmovieplayercontroller.html&ei=eFC2T6-YIsrHmAWb3s2wCQ&usg=AFQjCNH9oxbcw_p3DbGw4iUsygB48DZxrw&sig2=wd0HTWaHCN_pdSeYmbcweA)

	
  * [_MPMoviePlayerController_ 與MPMoviePlayerViewController ](http://zonble.net/archives/2010_07/1337.php)

	
  * [Iphone中利用_MPMoviePlayerController_線上播放視頻(轉) @ OPEN](http://fecbob.pixnet.net/blog/post/36635972-iphone%E4%B8%AD%E5%88%A9%E7%94%A8mpmovieplayercontroller%E7%B7%9A%E4%B8%8A%E6%92%AD%E6%94%BE%E8%A6%96%E9%A0%BB(%E8%BD%89)


