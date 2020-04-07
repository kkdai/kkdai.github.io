---
layout: post
layout: post
author: kkdai
comments: true
date: 2013-07-04 12:34:14+00:00
slug: ios-%e5%88%a9%e7%94%a8json%e7%9a%84%e6%96%b9%e5%bc%8f%e9%80%a3%e6%8e%a5google-drive%e4%b8%8a%e7%9a%84spreadsheet
title: '[IOS] 利用JSON的方式連接Google Drive上的Spreadsheet'
wordpress_id: 1259
categories:
- iOS的程式開發
---

在自學iOS程式的途中，總算需要一些server上的資料了，但是其實也只是要讀一些資料罷了
在需求相當的簡單之下，也曾經去尋找過CSV甚至是找個地方把資料寫成JSON硬讀
不過由於要能方便的修改，所以似乎使用Google Drive上面的Spreadsheet是最簡單最方便的方式

主要參考文章如下：



	
  * [ iOS中NSJSONSerialization解析JSON数据暨google地理信息处理案例](http://blog.csdn.net/mikixiyou/article/details/8692175)

	
    * 主要是看如何處理iOS5中使用基本的NSJSONSerialization來處理JSON 的資料




	
  * [JSON data from google spreadsheet](http://stackoverflow.com/questions/16230760/json-data-from-google-spreadsheet)

	
    * 主要是看如何把Google Drive裡面的資料分享出來並且變成JSON





流程如下：

	
  * 建立一個Google Spreadsheet

	
  * 選擇[Publish to web]

	
  * 勾選[Automatically republish when changes are made]

	
  * 會得到一個url～比如說是[https://docs.google.com/spreadsheet/pub?key=0An1-zUNFyMVLdEFEdVV3N2h1SUJOdTdKQXBfbGpNTGc&output=html](https://docs.google.com/spreadsheet/pub?key=0An1-zUNFyMVLdEFEdVV3N2h1SUJOdTdKQXBfbGpNTGc&output=html)

	
    * 其中重要的是 key=XXX 後面的資料




	
  * 將你的位置改成  [https://spreadsheets.google.com/feeds/list/XXXXXX1/public/values?alt=json](https://spreadsheets.google.com/feeds/list/0Artq5Bi16cQedE5mU21kdkJBWUtPU01XbS1uNW5JbEE/1/public/values?alt=json)  XXX換成你的key

	
  * 這樣就會得到一串JSON

	
  * 接下來就是程式碼的部份


主要程式都是參考[ iOS中NSJSONSerialization解析JSON数据暨google地理信息处理案例](http://blog.csdn.net/mikixiyou/article/details/8692175)

    
    -(void)parseJson
    {
    //The URL of JSON service
    //NSString *_urlString = @"http://maps.googleapis.com/maps/api/geocode/json?address=nanjing&sensor=true";
    NSString *_urlString = @"https://spreadsheets.google.com/feeds/list/0Artq5Bi16cQedE5mU21kdkJBWUtPU01XbS1uNW5JbEE/1/public/values?alt=json";
    NSString *_dataString = [[NSString alloc] initWithData:[_urlString dataUsingEncoding:NSASCIIStringEncoding allowLossyConversion:YES] encoding:NSASCIIStringEncoding];
    
    //_dataString=[NSString stringWithUTF8String:[_urlString UTF8String]];
    
    NSURL *_url = [NSURL URLWithString:_dataString];
    NSMutableURLRequest *_request = [NSMutableURLRequest requestWithURL:_url];
    [_request setValue:@"accept" forHTTPHeaderField:@"application/json"];
    [NSURLConnection sendAsynchronousRequest:_request
    queue:[NSOperationQueue mainQueue]
    completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
    //block define statment
    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
    
    int responseStatusCode = [httpResponse statusCode];
    NSLog(@"response status code is %d",responseStatusCode);
    
    NSError *_errorJson = nil;
    
    NSDictionary *resultsDic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:&error];
    
    if (_errorJson != nil)
    {
    NSLog(@"Error %@", [_errorJson localizedDescription]);
    }
    NSDictionary *resultDicFeed = [resultsDic objectForKey:@"feed"];
    NSArray *resultsArryEntry= [resultDicFeed objectForKey:@"entry"];
    for (NSDictionary * resultDetailDicAll in resultsArryEntry)
    {
    NSDictionary *resultsDicID=[resultDetailDicAll objectForKey:@"gsx$id"];
    NSString * dataID=[resultsDicID objectForKey:@"$t"];
    
    NSDictionary *resultsDicName=[resultDetailDicAll objectForKey:@"gsx$name"];
    NSString * dataName=[resultsDicName objectForKey:@"$t"];
    
    NSDictionary *resultsDicDesc=[resultDetailDicAll objectForKey:@"gsx$description"];
    NSString * dataDesc=[resultsDicDesc objectForKey:@"$t"];
    }
    
    }];
    }




如果你有其他欄位名稱～記得把欄位gsx$id gsx$name  gsx$description 改成你的欄位值....
