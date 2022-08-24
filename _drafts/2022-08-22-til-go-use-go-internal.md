---
layout: post
title: "[學習文件][Golang] 從 Go 原始碼裡面拆解出的好用套件 go-internal "
description: ""
category: 
- 學習文件
tags: ["Golang", "test-tool"]
---

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Released a new version of <a href="https://twitter.com/rogpeppe?ref_src=twsrc%5Etfw">@rogpeppe</a>&#39;s go-internal with some notable changes to testscript, used heavily in <a href="https://twitter.com/cue_lang?ref_src=twsrc%5Etfw">@cue_lang</a> :) A special thanks to <a href="https://twitter.com/bitfield?ref_src=twsrc%5Etfw">@bitfield</a>, <a href="https://twitter.com/tomwpayne?ref_src=twsrc%5Etfw">@tomwpayne</a>, and <a href="https://twitter.com/FiloSottile?ref_src=twsrc%5Etfw">@FiloSottile</a> for their recent contributions! <a href="https://twitter.com/hashtag/golang?src=hash&amp;ref_src=twsrc%5Etfw">#golang</a><a href="https://t.co/hDAqsecssu">https://t.co/hDAqsecssu</a></p>&mdash; Daniel Martí (@mvdan_) <a href="https://twitter.com/mvdan_/status/1561967605073723392?ref_src=twsrc%5Etfw">August 23, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

有一天這樣的 [tweet](https://twitter.com/mvdan_/status/1561967605073723392) 吸引到我，加上裡面有提到前Google 資安大神  [@FiloSottile](https://twitter.com/FiloSottile?ref_src=twsrc^tfw|twcamp^tweetembed|twterm^1561967605073723392|twgr^|twcon^s1_&ref_url=about%3Asrcdoc) 。 讓我認真的去了解一下什麼是 [Go-Internal](https://github.com/rogpeppe/go-internal) 

## 什麼是 Go-Internal 

#### 位置： [https://github.com/rogpeppe/go-internal]( https://github.com/rogpeppe/go-internal)

他是一個將 Go Internal source code 重新整理後，獨立出來使用的小套件。其實裡面有許多很好用的小工具可以使用。

## 關於 testscript

**testscript**: script-based testing based on txtar files

這樣看可能還不知道，裡面的 `testscript` 要怎麼使用。但是從 [Reddit](https://www.reddit.com/r/golang/comments/c67zv0/unit_testing_cobra/) 裡面的介紹蠻明白的。

```
testscript was factored out of the cmd/go internals where it is used to test the go command itself in various combinations/permutations.
```



## 相關文章：

- [Reddit: Unit Testing Cobra](https://www.reddit.com/r/golang/comments/c67zv0/unit_testing_cobra/)

- [Tweet about go-internal 1.9 release](https://twitter.com/mvdan_/status/1561967605073723392)

- 

