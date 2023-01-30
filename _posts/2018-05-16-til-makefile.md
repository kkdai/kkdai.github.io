---
layout: post
title: "[TIL][makefile] 一些常用的 Makefile 指令整理"
description: ""
category: 
- TodayILearn
- Makefile
tags: ["TIL", "makefile", "docker"]
---

# 摘要

 花了一點時間玩了一下 makefile ，有一些字串對應方式與使用方式在不使用 `sed` 的前提下其實也很方便． 順手整理一下一些常用的案例．

並且也列出一些使用 makefile 在編譯 Dockerfile 的時候經常會用到的範例．

# 基礎常見語法:


## wildcard 

透過 `wildcard` 可以擴展找出所有匹配的項目．

### 找出所有具有 Dockerfile 的目錄(並且依序排列)

`DOCKERFILE_PATHS := $(sort $(wildcard */Dockerfile))`


## patsubst 替換字串 

常用再換掉路徑，去除路徑．以下紀錄幾個常用的．

### 取得 Parent Path (ex:  a/b/c/d --> a/b/c)

`PARENT_PATHS := $(patsubst %/,%,$(dir $(CURRENT_PATHS)))`

## nodir 去除所有目錄 

這個可以幫你去除所有目錄，剩下檔名． ex: a/b/c/d  --> d

### 取得上一層目錄列表 

`PARENT_PATH_LIST := $(notdir $(dir $(CURRENT_PATHS)))`

這裡也要解釋一下， `dir` 會取得目錄而已．跟 `nodir` 剛好相反．

另外， `basename` 跟 `dir` 也不同．

`$(basename a/b/c/d.a) —>  d`

# 進階用法 `*, ` `%` 與`$*` 跟 `$<`

這邊講解起來會有點難懂，就用一個案例來講解．

- `$*` 會找出所有結果
- `%` 會將前方變數帶給之後用
- `$<` 列出前面的結果

以下用個例子

## 範例(1) : 找出 Dockerfile 並且編譯


```
build-image-%: %/Dockerfile $(shell find $* -type f)
	time docker build  \
		--tag docker.io/$(YOUR_PROJECT)/%:master \
		--file $< \
		$(dir $<)
```

這個會去編譯某個你輸入底下的 Dockerfile ．

舉例而言: 你有個目錄 `tensorflow/Dockerfile` 你就要輸入 `make build-image-tensorflow` ，他就會自動找出這個 Dockerfile 並且編譯名稱為 `$(YOUR_PROJECT)/tensorflow:master` 以下透過這個案例解釋每個用法:

- `%` 會將前面輸入的，帶到後面．在這裡也就是 `tensorflow` 帶到第二個與第三個出現`%`的地方 
- `$<` 會列出 `shell find $* -type f` 結果，也就是列出所有檔案，並且符合 `%/Dockerfile` 也就是如果你有其他檔案舉例為 `tensorflow/README.md` 也不會列出．


似乎這樣好像很麻煩? 其實透過前面介紹的基礎常見語法，可以更方便編譯．

## 範例(2) : 找出所有符合目錄直接帶到以上編譯

```
#find all docker files
DOCKERFILES := $(sort $(wildcard */Dockerfile))

#extract dir name
DIRS := $(patsubst %/Dockerfile,%,$(basename $(DOCKERFILES)))

#remain only dir name list
NAMES := $(notdir $(DIRS))

#add prefix combine to function strings
TARGETS := $(addprefix build-image-,$(NAMES))

#get all functions and run it.
all: $(TARGETS)
```

舉例而言，你有以下目錄結構

- a/README.md
- b/Dockerfile
- b/README.md
- c/Dockerfile
- d/test.c

你現在只要輸入 `make all` 就會自動把 b/Dockerfile 與 c/Dockerfile 找出來，並且組成字串 `build-image-b build-image-c` 然後呼叫到範例(1)來呼叫兩個 `docker build`

# Reference

- [8.2 Functions for String Substitution and Analysis](https://www.gnu.org/software/make/manual/html_node/Text-Functions.html)
- [Makefile的一些用法](http://deanjai.blogspot.tw/2008/02/objs-foo.html)
- [寫 Web 也可以用 Makefile：好好管理你的環境流程](https://blog.goodjack.tw/2023/01/use-makefile-to-manage-workflows-for-web-projects.html)