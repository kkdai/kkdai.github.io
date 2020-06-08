---
layout: post
title: "[TIL]關於 VIM 的錯誤訊息:  Exception not caught: [vim-hug-neovim-rpc] requires `:pythonx import neovim` command to work"
description: ""
category: 
- TodayILearn
- Podcast
tags: ["Golang", "WFH"]
---

## 問題
```
 Exception not caught: [vim-hug-neovim-rpc] requires `:pythonx import [pynvim | neovim]` command to work
```

每次只要透過 `brew upgrade` 來更新 vim ，就很容易發生以下的問題。（應該是跟更新了 python 有關）。

不論跑 `:pythonx import pynvim` 或是 `:pythonx import neovim` 都是一樣的。

找了很久才找到解答：



## 解答：

參考 [Error Every time I load in vim8 (not neovim)](https://github.com/roxma/vim-hug-neovim-rpc/issues/47)   可以看到以下解法：

- `brew link --overwrite python@3.8 --force`
- `pip3 install pynvim`



也可以透過:

`echo 'export PATH="/usr/local/opt/python@3.8/bin:$PATH"' >> ~/.zshrc`

來解決。



## 相關鏈結:

#### [Error Every time I load in vim8 (not neovim)](https://github.com/roxma/vim-hug-neovim-rpc/issues/47) 