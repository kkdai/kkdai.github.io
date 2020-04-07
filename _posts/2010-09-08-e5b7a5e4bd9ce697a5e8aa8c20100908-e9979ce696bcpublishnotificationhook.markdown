---
layout: post
layout: post
author: kkdai
comments: true
date: 2010-09-08 11:07:00+00:00
slug: '%e5%b7%a5%e4%bd%9c%e6%97%a5%e8%aa%8c20100908-%e9%97%9c%e6%96%bcpublishnotificationhook'
title: '工作日誌2010/09/08: 關於PublishNotificationHook'
wordpress_id: 1140
categories:
- Windows Live Writer的相關開發
---

自從寫完第一個[Hello World](http://wlwextensionlearning.blogspot.com/2010/09/windows-live-writer-plugin-world-sample.html)之後，本來緊接著就打算繼續把PublishNotificationHook放入原來的第一個[Hello World](http://wlwextensionlearning.blogspot.com/2010/09/windows-live-writer-plugin-world-sample.html)之中。

 

參考網路這篇文章([The New Live Writer SDK](http://scottisafooldev.spaces.live.com/blog/cns!FE151030F50B5B37!982.entry))裡面的source code，單純的把code加入之後，就像以下的狀態。

 

 

 

  
    
    <div><!--<br></br><br></br>Code highlighting produced by Actipro CodeHighlighter (freeware)<br></br>http://www.CodeHighlighter.com/<br></br><br></br>--><span style="color:#008080;"> 1</span> <span style="color:#0000ff;">using</span><span style="color:#000000;"> WindowsLive.Writer.Api;<br></br></span><span style="color:#008080;"> 2</span> <span style="color:#000000;"><br></br></span><span style="color:#008080;"> 3</span> <span style="color:#000000;"></span><span style="color:#0000ff;">namespace</span><span style="color:#000000;"> LiveWriterHelloWorld<br></br></span><span style="color:#008080;"> 4</span> <span style="color:#000000;">{<br></br></span><span style="color:#008080;"> 5</span> <span style="color:#000000;">    [WriterPluginAttribute<br></br></span><span style="color:#008080;"> 6</span> <span style="color:#000000;">      (</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;2f437bf1</span><span style="color:#000000;">-</span><span style="color:#000000;">fe57</span><span style="color:#000000;">-</span><span style="color:#000000;">41c8</span><span style="color:#000000;">-</span><span style="color:#000000;">931a</span><span style="color:#000000;">-</span><span style="color:#000000;">d20066ea174e</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;, </span><span style="color:#000000;">&</span><span style="color:#000000;">quot;Hello World</span><span style="color:#000000;">!</span><span style="color:#800080;">2</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;,<br></br></span><span style="color:#008080;"> 7</span> <span style="color:#000000;">        PublisherUrl </span><span style="color:#000000;">=</span><span style="color:#000000;"> </span><span style="color:#000000;">&</span><span style="color:#000000;">quot;http:</span><span style="color:#008000;">//</span><span style="color:#008000;">wlwextensionlearning.blogspot.com/&quot;,</span><span style="color:#008000;"><br></br></span><span style="color:#008080;"> 8</span> <span style="color:#008000;"></span><span style="color:#000000;">        Description </span><span style="color:#000000;">=</span><span style="color:#000000;"> </span><span style="color:#000000;">&</span><span style="color:#000000;">quot;Going to 2nd testing code</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;)]<br></br></span><span style="color:#008080;"> 9</span> <span style="color:#000000;">    [InsertableContentSourceAttribute(</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;Hello World</span><span style="color:#000000;">!&</span><span style="color:#000000;">quot;, SidebarText </span><span style="color:#000000;">=</span><span style="color:#000000;"> </span><span style="color:#000000;">&</span><span style="color:#000000;">quot;Hello World</span><span style="color:#000000;">!&</span><span style="color:#000000;">quot;)]<br></br></span><span style="color:#008080;">10</span> <span style="color:#000000;">    </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">class</span><span style="color:#000000;"> HelloWorldPlugin : ContentSource<br></br></span><span style="color:#008080;">11</span> <span style="color:#000000;">    {<br></br></span><span style="color:#008080;">12</span> <span style="color:#000000;">        </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">override</span><span style="color:#000000;"> DialogResult CreateContent(IWin32Window dialogOwner, </span><span style="color:#0000ff;">ref</span><span style="color:#000000;"> </span><span style="color:#0000ff;">string</span><span style="color:#000000;"> content)<br></br></span><span style="color:#008080;">13</span> <span style="color:#000000;">        {<br></br></span><span style="color:#008080;">14</span> <span style="color:#000000;">            content </span><span style="color:#000000;">=</span><span style="color:#000000;"> </span><span style="color:#000000;">&</span><span style="color:#000000;">quot;</span><span style="color:#000000;">&</span><span style="color:#000000;">lt;b</span><span style="color:#000000;">&</span><span style="color:#000000;">gt;Hello World</span><span style="color:#000000;">!&</span><span style="color:#000000;">lt;</span><span style="color:#000000;">/</span><span style="color:#000000;">b</span><span style="color:#000000;">&</span><span style="color:#000000;">gt;</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;;<br></br></span><span style="color:#008080;">15</span> <span style="color:#000000;"><br></br></span><span style="color:#008080;">16</span> <span style="color:#000000;">            </span><span style="color:#0000ff;">return</span><span style="color:#000000;"> DialogResult.OK;<br></br></span><span style="color:#008080;">17</span> <span style="color:#000000;">        }<br></br></span><span style="color:#008080;">18</span> <span style="color:#000000;">    }<br></br></span><span style="color:#008080;">19</span> <span style="color:#000000;">    </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">class</span><span style="color:#000000;"> PublishNotificationExample : PublishNotificationHook<br></br></span><span style="color:#008080;">20</span> <span style="color:#000000;">    {<br></br></span><span style="color:#008080;">21</span> <span style="color:#000000;">        </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">override</span><span style="color:#000000;"> </span><span style="color:#0000ff;">bool</span><span style="color:#000000;"> OnPrePublish(IWin32Window dialogOwner,<br></br></span><span style="color:#008080;">22</span> <span style="color:#000000;">        IProperties properties, IPublishingContext publishingContext,<br></br></span><span style="color:#008080;">23</span> <span style="color:#000000;">        </span><span style="color:#0000ff;">bool</span><span style="color:#000000;"> publish)<br></br></span><span style="color:#008080;">24</span> <span style="color:#000000;">        {<br></br></span><span style="color:#008080;">25</span> <span style="color:#000000;">            </span><span style="color:#008000;">//</span><span style="color:#008000;"> Check the post contents to see if liveside appears, if it does,             </span><span style="color:#008000;">//</span><span style="color:#008000;"> return true (publish), <br></br></span><span style="color:#008080;">26</span> <span style="color:#008000;">            </span><span style="color:#008000;">//</span><span style="color:#008000;"> if it doesn't, return false (cancel publish)</span><span style="color:#008000;"><br></br></span><span style="color:#008080;">27</span> <span style="color:#008000;"></span><span style="color:#000000;">            </span><span style="color:#0000ff;">return</span><span style="color:#000000;"> publishingContext.PostInfo.Contents.ToLower().Contains(</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;liveside</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;)
    ;<br></br></span><span style="color:#008080;">28</span> <span style="color:#000000;">        }<br></br></span><span style="color:#008080;">29</span> <span style="color:#000000;">        </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">override</span><span style="color:#000000;"> </span><span style="color:#0000ff;">void</span><span style="color:#000000;"> OnPostPublish(IWin32Window dialogOwner,<br></br></span><span style="color:#008080;">30</span> <span style="color:#000000;">               IProperties properties, IPublishingContext publishingContext,<br></br></span><span style="color:#008080;">31</span> <span style="color:#000000;">                </span><span style="color:#0000ff;">bool</span><span style="color:#000000;"> publish)<br></br></span><span style="color:#008080;">32</span> <span style="color:#000000;">        {<br></br></span><span style="color:#008080;">33</span> <span style="color:#000000;">            </span><span style="color:#008000;">//</span><span style="color:#008000;"> If this post is a draft (false), don't do anything<br></br></span><span style="color:#008080;">34</span> <span style="color:#008000;">            </span><span style="color:#008000;">//</span><span style="color:#008000;"> if it's an actual publish, then publish = true;</span><span style="color:#008000;"><br></br></span><span style="color:#008080;">35</span> <span style="color:#008000;"></span><span style="color:#000000;">            </span><span style="color:#0000ff;">if</span><span style="color:#000000;"> (</span><span style="color:#000000;">!</span><span style="color:#000000;">publish)<br></br></span><span style="color:#008080;">36</span> <span style="color:#000000;">                </span><span style="color:#0000ff;">return</span><span style="color:#000000;">;<br></br></span><span style="color:#008080;">37</span> <span style="color:#000000;"><br></br></span><span style="color:#008080;">38</span> <span style="color:#000000;">            </span><span style="color:#0000ff;">string</span><span style="color:#000000;"> updateTwitter </span><span style="color:#000000;">=</span><span style="color:#000000;"> </span><span style="color:#0000ff;">string</span><span style="color:#000000;">.Format(</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;{</span><span style="color:#800080;">0</span><span style="color:#000000;">} </span><span style="color:#000000;">-</span><span style="color:#000000;"> {</span><span style="color:#800080;">1</span><span style="color:#000000;">}</span><span style="color:#000000;">&</span><span style="color:#000000;">quot;,<br></br></span><span style="color:#008080;">39</span> <span style="color:#000000;">                publishingContext.PostInfo.Title,<br></br></span><span style="color:#008080;">40</span> <span style="color:#000000;">                publishingContext.PostInfo.Permalink);<br></br></span><span style="color:#008080;">41</span> <span style="color:#000000;"><br></br></span><span style="color:#008080;">42</span> <span style="color:#000000;">            </span><span style="color:#008000;">//</span><span style="color:#008000;"> Code to update Twitter</span><span style="color:#008000;"><br></br></span><span style="color:#008080;">43</span> <span style="color:#008000;"></span><span style="color:#000000;">        }<br></br></span><span style="color:#008080;">44</span> <span style="color:#000000;">    }<br></br></span><span style="color:#008080;">45</span> <span style="color:#000000;">}</span></div>






  




編譯完之後，會發現無法收到callback event… 持續探討([The New Live Writer SDK](http://scottisafooldev.spaces.live.com/blog/cns!FE151030F50B5B37!982.entry)) 的原始碼之後。 發現問題可能出在



  






  





  
    
    <div><!--<br></br><br></br>Code highlighting produced by Actipro CodeHighlighter (freeware)<br></br>http://www.CodeHighlighter.com/<br></br><br></br>--><span style="color:#008080;"> 1</span> <span style="color:#000000;">    </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">class</span><span style="color:#000000;"> HelloWorldPlugin : ContentSource<br></br></span><span style="color:#008080;"> 2</span> <span style="color:#000000;">    {<br></br></span><span style="color:#008080;"> 3</span> <span style="color:#000000;">        </span><span style="color:#0000ff;">public</span><span style="color:#000000;"> </span><span style="color:#0000ff;">override</span><span style="color:#000000;"> DialogResult CreateContent(IWin32Window dialogOwner, </span><span style="color:#0000ff;">ref</span><span style="color:#000000;"> </span><span style="color:#0000ff;">string</span><span style="color:#000000;"> content)<br></br></span><span style="color:#008080;"> 4</span> <span style="color:#000000;">        {<br></br></span><span style="color:#008080;"> 5</span> <span style="color:#000000;">            content </span><span style="color:#000000;">=</span><span style="color:#000000;"> </span><span style="color:#800000;">"</span><span style="color:#800000;"><b>Hello World!</b></span><span style="color:#800000;">"</span><span style="color:#000000;">;<br></br></span><span style="color:#008080;"> 6</span> <span style="color:#000000;"><br></br></span><span style="color:#008080;"> 7</span> <span style="color:#000000;">            </span><span style="color:#0000ff;">return</span><span style="color:#000000;"> DialogResult.OK;<br></br></span><span style="color:#008080;"> 8</span> <span style="color:#000000;">        }<br></br></span><span style="color:#008080;"> 9</span> <span style="color:#000000;">    }<br></br></span><span style="color:#008080;">10</span> <span style="color:#000000;"></span></div>





也就是主要是因為有implement ContentSource 的關係，造成無法收到相關的event。 這個主要原因可能有待詳細查看。當你改好並且把DLL 複製好之後你就會在你的blog 的plugin上面查看到。

  




![WLW0908.jpg](http://farm5.static.flickr.com/4132/4970597862_365576a678.jpg)



  






  






  




以上..
