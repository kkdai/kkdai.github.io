---
layout: post
title: "[TIL][makefile] make file 進階用法 (Secondary Expansion)"
description: ""
category: 
- TodayILearn
- Makefile
tags: ["TIL", "docker", "makefile"]
---

# 問題敘述

我有下列的服務要透過 make file 來編譯 Dockerfile ，由於那些服務有版本的區別，而且不同的版本都需要同時存在． 也就是說 app:v1  跟 app:v2 必需要同時能存在，並且要能夠讓 make file 能夠支援新的版本 app:v3 的產生． 舉例來說，我之後只要輸入 `make build-images` 就要能夠自動跑出 app1-v1, app1-v2, app2-v1 並且能夠根據這些 build target 自動 build 相關的 docker image



- build
  - app1
    - v1
      - Dockerfile
    - v2
      - Dockerfile
  - app2
    - v1
      - Dockerfile



# 如何得到 build target

首先我們要能搜集所有的 build targets ，透過一系列的 build target 再來跑相關的 make process.

```makefile
BUILD_DOCKERFILES := $(sort $(wildcard build/*/*/Dockerfile))
```

這一段是找到所有具有 Dockerfile 的檔案.. 在這裡會找出...  

- build/app1/v1/Dockerfile
- build/app1/v2/Dockerfile
- build/app2/v1/Dockerfile

這時候我們要做一點小處理，要去除 `Dockerfile` 這個不需要的檔案名稱．這個要透過上一篇提過的 `%` 來處理． 快速講解一下，下面的例子是說，不論何種字串只要是 xxxx/Dockerfile 都會被取代成 xxxx

```makefile
BUILD_DIRS := $(patsubst %/Dockerfile,%,$(BUILD_DOCKERFILES))
```

這時候 `BUILD_DIRS` 就會轉換為: 

- build/app1/v1
- build/app1/v2
- build/app2/v1

再來，我們要整理成 `build-app-v1` 的格式，這個可以透過 `subst` 來替代

```makefile
BUILD_APP_VER := $(subst /,-,$(BUILD_DIRS))
```

這樣就會得到:

- build-app1-v1
- build-app1-v2
- build-app2-v1

最後，我們要其轉換成 app1-v1, app1-v2 ... 一樣透過 `%` 來替換

```makefile
BUILD_NAMES := $(patsubst build-%,%,$(BUILD_APP_VER))
```



最後我們要得到 build target 希望是 build-app1-v1, build-app1-v2...

```makefile
BUILD_TARGETS := $(addprefix notebook-image-,$(BUILD_NAMES))
```

這樣就可以得到我們要的結果．

最後整理一下...  

```makefile
BUILD_DOCKERFILES := $(sort $(wildcard build/*/*/Dockerfile))
BUILD_DIRS := $(patsubst %/Dockerfile,%,$(BUILD_DOCKERFILES))
BUILD_APP_VER := $(subst /,-,$(BUILD_DIRS))
BUILD_NAMES := $(patsubst build-%,%,$(BUILD_APP_VER))
BUILD_TARGETS := $(addprefix notebook-image-,$(BUILD_TARGETS))
```

# 開始撰寫 build target 本體要做的事情

這時候因為我們得到 `build-app1-v1`, `build-app1-v2` 跟 `build-app2-v1`. 我們就要來展開要做的事情．我們要透過展開 `build-app1-v1` 來編譯出 `docker.io/evanlin/app:v1` 的 docker tag image

## 一個錯誤的範例 

```makefile
build-%: build/$(subst -,/,%)/Dockerfile $(shell find build -type f)
...
```

先解釋一下，在 `build-%:` 後面的就是條件式 *prerequisites* ，也就是必須要符合條件內才會繼續往下執行．在這裡條件是 透過找所有 `build` 子目錄所有檔案，來確認是否有符合 `build/app1/v1/Dockerfile` 的檔案．

起出這樣看起來很正常，我們想要透過替換字串來讓 `%` 裡面的內容來修改 `app1-v1` 為 `app1/v1` 但是這時候會發現無法替換．

# Secondary Expansion

經過查詢 [GNU Make: Secondary Expansion](https://www.gnu.org/software/make/manual/html_node/Secondary-Expansion.html) 可以達成我的需求．所以修改如下:

```makefile
.SECONDEXPANSION:
build-%: build/$$(subst -,/,%)/Dockerfile $(shell find build -type f)
...
```

這樣就能夠找到，也能夠繼續之後的處理．

# Reference

- [GNU Make: 8.2 Functions for String Substitution and Analysis](https://www.gnu.org/software/make/manual/html_node/Text-Functions.html)
- [Stackoverflow: How to replace string in “%” in make file](https://stackoverflow.com/questions/50494899/how-to-replace-string-in-in-make-file/50495957#50495957) 
- [Makefile中的 \$ 和 \$\$](https://blog.csdn.net/qq_34369618/article/details/52973720) 
- [GNU Make: Secondary Expansion](https://www.gnu.org/software/make/manual/html_node/Secondary-Expansion.html)
- [GNU Make: Automatic Variables](https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html)