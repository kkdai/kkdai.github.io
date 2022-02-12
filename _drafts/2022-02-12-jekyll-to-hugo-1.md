---
layout: post
title: "[學習文件] 將 Jekyll migrate 到 Hugo 流程 (一) "
description: ""
category: 
- 學習文件
tags: ["Jekyll", "Hugo", "Blog"]
---



# 前言:



# 流程：

## 快速整合

```
brew install hugo
```



```
mkdir migrate
hugo import jekyll jing migrate
```



```
$ hugo import jekyll migrate_hugo dest                                                                           Importing...
Congratulations! 1289 post(s) imported!
Now, start Hugo by yourself:
$ git clone https://github.com/spf13/herring-cove.git dest/themes/herring-cove
$ cd dest
$ hugo server --theme=herring-cove
  ~  git clone https://github.com/spf13/herring-cove.git dest/themes/herring-cove                                      2979  23:00:41 
正複製到 'dest/themes/herring-cove'...

remote: Enumerating objects: 178, done.
remote: Total 178 (delta 0), reused 0 (delta 0), pack-reused 178
接收物件中: 100% (178/178), 6.57 MiB | 1.49 MiB/s, 完成.
處理 delta 中: 100% (71/71), 完成.
```



```
.gitmodules 

[submodule "themes/m10c"]
	path = themes/m10c
	url = https://github.com/vaga/hugo-theme-m10c.git
```



```
baseURL = "/"
languageCode = "en-us"

title = "Blog E"
name = "Evan Lin"
description = "Attitude is everything"

theme = "m10c"
paginate = "10"
```




## 相關文章：

- [Migrating from Jekyll to Hugo](https://chenhuijing.com/blog/migrating-from-jekyll-to-hugo/#%F0%9F%91%9F)
