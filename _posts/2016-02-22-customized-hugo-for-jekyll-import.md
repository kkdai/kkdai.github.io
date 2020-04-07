---
layout: post
title: "[Hugo] Customized hugo for jekyll import"
description: ""
category: 
- golang
tags: [jekyll, hugo, golang]
---

## Issue

hugo import jekyll is very easy to import your blog post from jekyll to hugo. 

However it has some issue, original you have change you permallink ex like:

- `permallink: /:title/` 
- `permallink: /:categories/:title/` 

I just file a [issue#1887](https://github.com/spf13/hugo/issues/1887) on Hugo project, detail as follow:


I try to use it to convert my blog from jekyll which run over decade and contain over 1000+ posts.
I found it will force to add "url" in every post which is /year/month/dat/postname.

for ex:

```
---
date: 2016-02-18T00:00:00Z
description: ""
tags: []
title: '[Visual Studio] Using Visual Studio 2015 to remote debugging C++ on linux'
url: /2016/02/18/vs-debug-linux-exe/
---
```

However it will conflict with my configuration of "permallink", I found it force to store the same "url" no matter your jekyll "permallink".

Here should be two approach of "url" generation rule:

- Load config from jekyll and parse "permallink" configuration to apply it.
- Just not use "url", let user to configuration whatever they want. (It might be some issue because the "title" in jekyll permalink, mean the "postname" in hugo but no permalink config with it.)
Any idea of this issue?


## Temprary Fix:

Because my jekyll config use `permallink: /:title/`, I directory modify source code to use `postName` to do correctly import.

### How to use it

```
go get github.com/kkdai/hugo  //if you don't install it before.
//build or run hugo on github.com/kkdai/hugo
```

## TODO:

I will try to survey how to import correctly jekyll configuration. However I still wait any comment on [issue #1887](https://github.com/spf13/hugo/issues/1887).


I also found hugo import jekyll will occur categories lost randomlly, keep tracking on this.