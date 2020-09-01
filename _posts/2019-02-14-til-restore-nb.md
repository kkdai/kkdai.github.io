---
layout: post
title: "[TIL] 如何快速重置上手習慣的 MacOSX 環境"
description: ""
category: 

- TodayILearn
- vscode
tags: ["TIL", "vscode", "GitHub"]
---

# 摘要

就在今年情人節，整台筆電就這樣爆炸了。 （霹靂星球....) 整個電池忽然無法充電，導致我只能盡快的麻煩公司的同事借來一台備用電腦。 但是整個使用習慣實在很痛苦，導致還是開了一個新的使用者將所有常用的設定都恢服。

這裡快速紀錄一下，我做了哪些事情。有興趣的人也可以參考我的設定。



# 基礎開發環境

先快速裝幾個需要的工具

- [Homebrew](https://brew.sh/) 列幾個會用的
  - vim
    - Install Vundle first`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`
    - vim 設定 https://github.com/kkdai/vimrc
  - go
  - zsh (等等會提到)
  - heroku (heroku cli 超常用)
	  - `brew tap heroku/brew && brew install heroku`
- [VSCode](https://code.visualstudio.com/) (離不開了)，順便列一下最少需要的 plugin

  - [Gitlens](https://github.com/eamodio/vscode-gitlens) 基本上懶得打 git 或是查資料用
  - [Go](https://github.com/Microsoft/vscode-go) 這不用問
  - Run vscode from your terminal https://code.visualstudio.com/docs/setup/mac
- [typora 寫部落格用的工具](https://typora.io/) ，之前有寫過[推廣文](http://www.evanlin.com/til-mdeditor-typora/)
- [PasteApp](https://pasteapp.me/) 方便你複製貼上的工具，免費七天。買下去你絕對不會後悔的。



# 順手的東西 zsh + oh-my-zsh

這邊簡單多了，參考這篇好文章 ["超簡單！十分鐘打造漂亮又好用的 zsh command line 環境"](https://medium.com/statementdog-engineering/prettify-your-zsh-command-line-prompt-3ca2acc967f)，條列出我有用到的：

- **iTerm2**

  - `brew tap caskroom/cask`
  - `brew cask instal iterm2`
  - Use `iTerm2-Color-Schemes `https://github.com/mbadolato/iTerm2-Color-Schemes

- #### **powerline font**

  - ``` 
    brew tap homebrew/cask-fonts
    ```
  - ```
    brew cask install font-source-code-pro
    ```
  - "SourceCodePro Nerd Font", 18, 

- **ZSH**

  - ```
    brew install zsh
    ```
- Install **oh-my-zsh**

  - ```
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```

- Install zsh integration for iTerm2

  - Refer https://www.iterm2.com/documentation-shell-integration.html
  - `curl -L https://iterm2.com/shell_integration/zsh \
  -o ~/.iterm2_shell_integration.zsh`
  - `source ~/.iterm2_shell_integration.zsh`

- Install PowerLevel9k

  - `git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k`

- 復原 zsh configuration 

  - https://github.com/kkdai/zsh

  - Install related plugin

    - ```
      git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
      git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
      git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
      ```
    
  - 有些用不到的可以先不要
    - miniconda
    - kubetcl

- 讓 vscode terminal 顯示 zsh  http://www.evanlin.com/til-zsh/ 

  - `user.setting` (cmd+,) 

- ```
  "terminal.integrated.shell.osx": "zsh",
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.fontSize": 12,
  ```



# 電腦電池壞掉我學到什麼

- 就算是 blog 文章還沒寫文，每天進度也要放上 github 
  - 原本只有 local commit 忘記 push
- "A git push a day, keep your computer safe anyway"



# Reference

- https://medium.com/statementdog-engineering/prettify-your-zsh-command-line-prompt-3ca2acc967f
- https://github.com/kkdai/zsh

```