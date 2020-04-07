---
layout: post
layout: post
title: "[Mac OSX] 一些在console的快速鍵設定"
description: ""
category: 
- 電腦操作
tags: [macosx, console]
---


## 前言:

主要是紀錄一下一些在命令列(console mode)下的快捷鍵，希望以後有需要用到可以給自己紀錄一下．


### Console的快捷鍵列表


參考[這裡](http://stackoverflow.com/questions/81272/is-there-any-way-in-the-os-x-terminal-to-move-the-cursor-word-by-word)來的．

以下兩個主要是一次移動一個字

- alt ⌥+F to jump Forward by a word
- alt ⌥+B to jump Backward by a word


這邊有其他的部分，移動字元，或是移動到一開始．

- ctrl+A to jump to start of the line
- ctrl+E to jump to end of the line
- ctrl+K to kill the line starting from the cursor position
- ctrl+Y to paste text from the kill buffer
- ctrl+R to reverse search for commands you typed in the past from your history
- ctrl+S to forward search (works in zsh for me but not bash)
- ctrl+F to move forward by a char
- ctrl+B to move backward by a char


### 在iTerm2 裡面編輯快捷鍵來使用一次跳一個字

參考[這裡](https://coderwall.com/p/h6yfda/use-and-to-jump-forwards-backwards-words-in-iterm-2-on-os-x)

- 按下 `cmd` + `,`
- 選取  `Profile` -> `Keys`
- 加入以下: 使用 `Alt`+ `<-`來向左跳一個字
	- Keyboard Shortcut: ⌥←
	- Action: Send Escape Sequence
	- Esc+: b
- 加入以下: 使用 `Alt`+ `->`來向右跳一個字
 	- Keyboard Shortcut: ⌥→
	- Action: Send Escape Sequence
	- Esc+: f
