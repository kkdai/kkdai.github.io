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
    - vim 設定 [https://github.com/kkdai/vimrc](https://github.com/kkdai/vimrc)
  - go
  - zsh (等等會提到)
  - heroku (heroku cli 超常用)
	  - `brew tap heroku/brew && brew install heroku`
- [VSCode](https://code.visualstudio.com/) (離不開了)，順便列一下最少需要的 plugin

  - [Gitlens](https://github.com/eamodio/vscode-gitlens) 基本上懶得打 git 或是查資料用
  - [Go](https://github.com/Microsoft/vscode-go) 這不用問
  - Run vscode from your terminal [https://code.visualstudio.com/docs/setup/mac](https://code.visualstudio.com/docs/setup/mac)
  - 關於設定部分，其實可以登入 Settings Sync 來[儲存設定](https://code.visualstudio.com/docs/editor/settings-sync)。
- [typora 寫部落格用的工具](https://typora.io/) ，之前有寫過[推廣文](http://www.evanlin.com/til-mdeditor-typora/)
- [PasteApp](https://pasteapp.me/) 方便你複製貼上的工具，免費七天。買下去你絕對不會後悔的。
- **Vim 相關安裝**
  - [GitHub.com/kkdai/vimrc](https://GitHub.com/kkdai/vimrc)
  - [https://github.com/tpope/vim-pathogen](https://github.com/tpope/vim-pathogen)
  - [https://github.com/preservim/nerdtree](https://github.com/preservim/nerdtree)
  - `:PluginInstall`



## 漂亮的字型很重要

![](https://github.com/ryanoasis/nerd-fonts/raw/master/images/nerd-fonts-logo.svg)

```
> git clone https://github.com/ryanoasis/nerd-fonts
> cd nerd-fonts
> ./install.sh
```



# 

# 順手的東西 zsh + oh-my-zsh

這邊簡單多了，參考這篇好文章 ["超簡單！十分鐘打造漂亮又好用的 zsh command line 環境"](https://medium.com/statementdog-engineering/prettify-your-zsh-command-line-prompt-3ca2acc967f)，條列出我有用到的：

- **iTerm2**

  - `brew tap caskroom/cask`
  - `brew cask instal iterm2`
  - Use `iTerm2-Color-Schemes ` [https://github.com/mbadolato/iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)

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

  - Refer [https://www.iterm2.com/documentation-shell-integration.html](https://www.iterm2.com/documentation-shell-integration.html)
  - `curl -L https://iterm2.com/shell_integration/zsh \
  -o ~/.iterm2_shell_integration.zsh`
  - `source ~/.iterm2_shell_integration.zsh`

- Install PowerLevel9k

  - `git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k`

- 復原 zsh configuration 

  - [https://github.com/kkdai/zsh](https://github.com/kkdai/zsh)

  - Install related plugin

    - ```
      git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
      git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
      git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
      ```
    
  - 有些用不到的可以先不要
    - miniconda
    - kubetcl

- 讓 vscode terminal 顯示 zsh  [http://www.evanlin.com/til-zsh/](http://www.evanlin.com/til-zsh/ ) 

  - `user.setting` (cmd+,) 

- ```
  "terminal.integrated.shell.osx": "zsh",
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.fontSize": 12,
  ```



# 我有兩個 git 網站需要兩把 key 一個 email 是  user@email1.com  user@email2.com  我該怎麼產生這兩把 key 然後可以使用 SSH 登入？

要為兩個不同的 Git 網站（例如 GitHub、GitLab 等）生成兩把 SSH 密鑰，並分別使用不同的電子郵件地址（user@email1.com 和 user@email2.com）進行 SSH 登錄，你可以按照以下步驟操作。這些步驟假設你使用的是 Linux、macOS 或 Windows（使用 Git Bash 或 WSL）。

### 步驟 1：生成兩把 SSH 密鑰

1. 打開終端

   1. 在 Linux/macOS 上使用終端。
   2. 在 Windows 上使用 Git Bash 或 WSL。

2. 為第一個電子郵件生成第一把密鑰

   `ssh-keygen -t ed25519 -C "user@email1.com" -f ~/.ssh/id_ed25519_email1`

   - -t ed25519：指定使用 Ed25519 算法（更安全且高效）。
   - -C "user@email1.com"：添加註釋（這裡是你的電子郵件）。
   - -f ~/.ssh/id_ed25519_email1：指定生成的密鑰文件路徑（私鑰為 id_ed25519_email1，公鑰為 id_ed25519_email1.pub）。
   - 按提示設置密碼（可選，建議設置以增加安全性）。

3. 為第二個電子郵件生成第二把密鑰

   `ssh-keygen -t ed25519 -C "user@email2.com" -f ~/.ssh/id_ed25519_email2`

4. 檢查生成的密鑰： 

   1. 在  `~/.ssh/`  目錄下，你應該會看到以下文件：

      `id_ed25519_email1 id_ed25519_email1.pub id_ed25519_email2 id_ed25519_email2.pub`

### 步驟 2：配置 SSH 客戶端

為了讓 SSH 知道如何根據不同的 Git 網站使用不同的密鑰，你需要創建或編輯 SSH 配置文件。

1. 創建或編輯 SSH 配置文件

   1. `touch ~/.ssh/config chmod 600 ~/.ssh/config`

   2. 用編輯器（例如 nano 或 vim）打開 ``~/.ssh/config`

   3. 添加以下配置

```
# 第一個 Git 網站（例如 GitHub）
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_email1
    IdentitiesOnly yes

# 第二個 Git 網站（例如 GitLab）
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/id_ed25519_email2
    IdentitiesOnly yes
```

   - Host：自定義一個別名（例如 github.com 或 gitlab.com）。
   - HostName：實際的域名。
   - User：Git 服務通常使用 git 作為用戶名。
   - IdentityFile：指定對應的私鑰文件。
   - IdentitiesOnly yes：確保只使用指定的密鑰。

2. **保存並退出**。

------

### 步驟 3：將公鑰添加到 Git 網站

1. 複製公鑰到剪貼板

   1. `cat ~/.ssh/id_ed25519_email1.pub`

2. 對於第二把密鑰：

  1. `cat ~/.ssh/id_ed25519_email2.pub`

3. 添加到 Git 網站

   - GitHub

     1. 登錄到 GitHub，進入 **Settings** > **SSH and GPG keys** > **New SSH key** 或 **Add SSH key**。
     2. 粘貼第一把公鑰（id_ed25519_email1.pub），給它一個描述性名稱（例如 email1-key）。
     
   - GitLab

     （或其他網站）：

     1. 登錄到 GitLab，進入 **User Settings** > **SSH Keys**。
     2. 粘貼第二把公鑰（id_ed25519_email2.pub），給它一個描述性名稱。

   - 根據網站的具體要求保存。

------

### 步驟 4：啟動 SSH Agent 並添加密鑰

1. 啟動 SSH Agent`eval "$(ssh-agent -s)"`

2. 添加私鑰到 SSH Agent `ssh-add ~/.ssh/id_ed25519_email1 ssh-add ~/.ssh/id_ed25519_email2`

   如果設置了密碼，會提示你輸入。

3. 檢查已添加的密鑰 `ssh-add -l`

   你應該能看到兩把密鑰的指紋。

### 步驟 5：測試 SSH 連接

1. 測試第一個 Git 網站 `ssh -T git@github.com`

   1. 如果配置正確，你會看到類似以下的輸出：

   2. Hi username! You've successfully authenticated...`

2. 測試第二個 Git 網站 `ssh -T git@gitlab.com`

   同樣應該返回成功信息。



# 電腦電池壞掉我學到什麼

- 就算是 blog 文章還沒寫文，每天進度也要放上 github 
  - 原本只有 local commit 忘記 push
- "A git push a day, keep your computer safe anyway"



# Reference

- [https://medium.com/statementdog-engineering/prettify-your-zsh-command-line-prompt-3ca2acc967f](https://medium.com/statementdog-engineering/prettify-your-zsh-command-line-prompt-3ca2acc967f)
- [https://github.com/kkdai/zsh](https://github.com/kkdai/zsh)

```

```