---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-03-11 01:11:38+00:00
slug: ios-%e9%97%9c%e6%96%bc%e6%99%82%e9%96%93nsdate%e7%9a%84%e7%9b%b8%e9%97%9c%e8%99%95%e7%90%86
title: '[iOS] 關於時間NSDate的相關處理'
wordpress_id: 1348
categories:
- iOS的程式開發
- 學習文件
---

昨天把NSDate的一些處理稍微研究了一下，有一些處理方式可以整理一下:




**1. 比較時間 (NSDate comparison)**** **



    
    <code style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;font-family:Consolas, Menlo, Monaco, 'Lucida Console', 'Liberation Mono', 'DejaVu Sans Mono', 'Bitstream Vera Sans Mono', 'Courier New', monospace, serif;white-space:inherit;"><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#00008b;" class="kwd">if</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">([</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pln">date1 compare</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">:</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pln">date2</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">]</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">==</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;" class="typ">NSOrderedDescending</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">)<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">{<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;" class="typ"> NSLog</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">(@</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#800000;" class="str">"date1 is later than date2"</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">);<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">}<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#00008b;" class="kwd">else</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#00008b;" class="kwd">if</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">([</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pln">date1 compare</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">:</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pln">date2</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">]</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">==</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;" class="typ">NSOrderedAscending</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">)<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">{<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;" class="typ"> NSLog</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">(@</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#800000;" class="str">"date1 is earlier than date2"</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">);<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">}<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#00008b;" class="kwd">else</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">{<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#2b91af;" class="typ"> NSLog</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">(@</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;color:#800000;" class="str">"dates are the same"</span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">);<br></br></span><span style="margin:0;padding:0;border:0;font-size:13.63636302948px;vertical-align:baseline;background-color:transparent;" class="pun">}</span></code>




**可以拿來比較時間的先後，在NSDate大概算是最方便的．**




**參考: [http://stackoverflow.com/questions/5965044/how-to-compare-two-nsdates-which-is-more-recent](http://stackoverflow.com/questions/5965044/how-to-compare-two-nsdates-which-is-more-recent)**




**  
**




**2. 時區的轉換**




使用NSDate拿來的時間都是GMT要顯示的時候得轉成當地時區(台灣是TST)




-(NSString*) transferGMT2TST: (NSDate*)inGMT :(NSString*) newTZ




{




    NSDateFormatter *dateFormatter = [[NSDateFormatteralloc] init];




    dateFormatter.dateFormat = @"yyyy-MM-dd'T'HH:mm";




    




    NSTimeZone *nTZ = [NSTimeZonetimeZoneWithAbbreviation:newTZ];




    [dateFormatter setTimeZone:nTZ];




    return [dateFormatter stringFromDate:inGMT];




}




 




3. 也可以把NSDate當成一個時間比對或是timer，可以先設定一個未來的時間，然後做時間的比較．




 




繼續研究....
