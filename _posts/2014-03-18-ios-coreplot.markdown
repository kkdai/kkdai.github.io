---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-18 09:55:31+00:00
slug: ios%e6%9c%80%e5%a4%9a%e4%ba%ba%e7%94%a8%e7%9a%84%e5%87%bd%e5%bc%8fcoreplot%e8%a8%ad%e5%ae%9a%e8%88%87%e4%bd%bf%e7%94%a8
title: '[iOS]繪圖函式CorePlot設定與使用'
wordpress_id: 1353
categories:
- iOS的程式開發
- 學習文件
---

[CorePlot](https://code.google.com/p/core-plot/) 是一個相當好用來畫各種條狀圖，折線圖與圓餅圖的library． 要在手機上畫出美美的條狀圖，圓餅圖與長方圖？真的只能靠這套Library了．  
以下整理一些在使用上可能會遇到的問題 




遇到的問題：

* “Undefined symbols for architecture x86_64”
    * [http://stackoverflow.com/questions/18408531/xcode-build-failure-undefined-symbols-for-architecture-x86-64](http://stackoverflow.com/questions/18408531/xcode-build-failure-undefined-symbols-for-architecture-x86-64)
        * 主要就是你拿到的library太舊了，可以去官方網站去找最新的來用
    * Cannot find XXX.h
        * 主要要記得改include path，這倒是跟使用其他library不常看到的設定選項．


使用了好用的[CorePlot](https://code.google.com/p/core-plot/)也必須要了解使用最基本的方式來畫圖  
以下是一些基本跟UIView 有關的部分，順便複習一下

* 關於UIView
* UIView Constructor 有兩種
    * - (id)initWithCoder:(NSCoder *)aDecoder  
    * - (id)initWithFrame:(CGRect)frame
    * initWithCoder 是給 StoryBoard 上面的UIView 使用，另外一個通常是自定frame大小的時候會用到.
    * 參數定義使用property與synthesize
* 要畫Label 與畫出有顏色的自定UIView 可以用以下方式

            // 畫出UILable  
            CGRect frame = CGRectMake(20, 45, 140, 21);
            UILabel *label = [[UILabel alloc] initWithFrame:frame];
            [self.view addSubview:label];
            [label setText:@"Number of sides:"];
        
            // 自定UIView但是是個塗滿的黑筐
            UIView *lineView = [[UIView alloc] initWithFrame:CGRectMake(0, 200, self.view.bounds.size.width, 30)];
        
            lineView.backgroundColor = [UIColorblackColor];
            [self.view addSubview:lineView];


* 參考：
    * [http://greenchiou.blogspot.tw/2011/04/ios-sdk-quartz-2d-0.html](http://greenchiou.blogspot.tw/2011/04/ios-sdk-quartz-2d-0.html)
    * [http://www.raywenderlich.com/1768/uiview-tutorial-for-ios-how-to-make-a-custom-uiview-in-ios-5-a-5-star-rating-view](http://www.raywenderlich.com/1768/uiview-tutorial-for-ios-how-to-make-a-custom-uiview-in-ios-5-a-5-star-rating-view)

* 設定動畫 Animation
    * 主要是利用 UIView 的transform 裡面主要有三種 (可以達到放大，縮小，旋轉與Translation)
      * CGAffineTransformMakeScale
      * CGAffineTransformMakeRotation  
      * CGAffineTransformMakeTranslation
    * 詳細內容如下:


            -(IBAction)startScaleTransform:(id)sender
            {
                self.view.transform = CGAffineTransformMakeScale(2,2);
                [UIViewbeginAnimations:nilcontext:NULL];
                [UIViewsetAnimationDuration:1];
                self.view.transform = CGAffineTransformMakeScale(1,1);
                self.view.alpha = 1.0;
                [UIViewcommitAnimations];
            }


**參考文章：**


  * CorePlot Github: [https://github.com/core-plot/core-plot](https://github.com/core-plot/core-plot)
  * CorePlot官方設定: [https://code.google.com/p/core-plot/wiki/UsingCorePlotInApplications](https://code.google.com/p/core-plot/wiki/UsingCorePlotInApplications)
  * 設定安裝中文: [http://www.cnblogs.com/lovecode/archive/2012/02/11/2346389.html](http://www.cnblogs.com/lovecode/archive/2012/02/11/2346389.html)
  * 設定安裝英文: [http://tech.pro/tutorial/939/using-core-plot-in-an-iphone-application](http://tech.pro/tutorial/939/using-core-plot-in-an-iphone-application)
  * CorePlot安裝與使用導覽(英文):
      * Pie Chart [http://www.raywenderlich.com/13269/how-to-draw-graphs-with-core-plot-part-1](http://www.raywenderlich.com/13269/how-to-draw-graphs-with-core-plot-part-1)
      * Bar Chart [http://www.raywenderlich.com/13271/how-to-draw-graphs-with-core-plot-part-2](http://www.raywenderlich.com/13271/how-to-draw-graphs-with-core-plot-part-2)
