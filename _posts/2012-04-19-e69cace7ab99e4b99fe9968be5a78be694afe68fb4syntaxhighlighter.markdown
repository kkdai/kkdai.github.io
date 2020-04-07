---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-04-19 11:36:59+00:00
slug: '%e6%9c%ac%e7%ab%99%e4%b9%9f%e9%96%8b%e5%a7%8b%e6%94%af%e6%8f%b4syntaxhighlighter'
title: 本站也開始支援SyntaxHighlighter
wordpress_id: 1156
categories:
- 柴米油鹽的東西
---

SyntaxHighlighter是一個在網站上相當好用的東西。我終於也在<a href="http://www.evanlin.comblog">www.evanlin.comblog</a>把他安裝好了<br></br>不過呢~ 由於Window Live Writer 還沒有找到真的讓我覺得很好用的部分<br></br>這個<a href="http://wlwsyntaxhighlighter.codeplex.com/">Plugin</a> 不好用(更正: 因為不支援最新版本的Syntaxhighlighter 3.0.8.3)<br></br>以下是我加入到MT 模板的code(不過這是用<a href="http://plugins.live.com/writer/detail/paste-from-visual-studio">VSpaste</a>)先貼的




    
      <span style="color:green;"><!-- Include required JS files -->
        </span><span style="color:blue;"><</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript" </span><span style="color:red;">src</span><span style="color:blue;">="js/shCore.js"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
        <</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript" </span><span style="color:red;">src</span><span style="color:blue;">="js/shBrushJScript.js"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
        <</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript" </span><span style="color:red;">src</span><span style="color:blue;">="js/shBrushCSharp.js"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
        <</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript" </span><span style="color:red;">src</span><span style="color:blue;">="js/shBrushCss.js"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
        <</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript" </span><span style="color:red;">src</span><span style="color:blue;">="js/shBrushJava.js"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
        <</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript" </span><span style="color:red;">src</span><span style="color:blue;">="js/shBrushXml.js"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
    
        <</span><span style="color:#a31515;">link </span><span style="color:red;">href</span><span style="color:blue;">="css/shCore.css" </span><span style="color:red;">rel</span><span style="color:blue;">="stylesheet" </span><span style="color:red;">type</span><span style="color:blue;">="text/css" />
        <</span><span style="color:#a31515;">link </span><span style="color:red;">href</span><span style="color:blue;">="css/shThemeDefault.css" </span><span style="color:red;">rel</span><span style="color:blue;">="stylesheet" </span><span style="color:red;">type</span><span style="color:blue;">="text/css" />
        </span><span style="color:green;"><!-- Include required JS files -->
    
        </span><span style="color:blue;"><</span><span style="color:#a31515;">script </span><span style="color:red;">type</span><span style="color:blue;">="text/javascript">
            </span><span style="color:#a31515;">SyntaxHighlighter.all()
        </span><span style="color:blue;"></</span><span style="color:#a31515;">script</span><span style="color:blue;">>
    </span>





話說~ [VSpaste](http://plugins.live.com/writer/detail/paste-from-visual-studio) 這個plugin 也很好用~ 跨Blog又不用安裝~ 





不過可能支援語系會少一點





以下是使用這個[Windows Live Writer Plugin(Windows Live Writer Source Code plugin for SyntaxHighlighter)](http://sourcecodeplugin.codeplex.com/) , 而且也支援最新版本的syntaxhighlighter 3.0.8.3








    
      <!-- Include required JS files -->
        <script type="text/javascript" src="js/shCore.js"></script>
        <script type="text/javascript" src="js/shBrushJScript.js"></script>
        <script type="text/javascript" src="js/shBrushCSharp.js"></script>
        <script type="text/javascript" src="js/shBrushCss.js"></script>
        <script type="text/javascript" src="js/shBrushJava.js"></script>
        <script type="text/javascript" src="js/shBrushXml.js"></script>
    
        <link href="css/shCore.css" rel="stylesheet" type="text/css" />
        <link href="css/shThemeDefault.css" rel="stylesheet" type="text/css" />
        <!-- Include required JS files -->
    
        <script type="text/javascript">
            SyntaxHighlighter.all()
        </script>
