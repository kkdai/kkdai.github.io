---
layout: post
title: "[Claude Code 實戰] 重新盤點終端機工作流：從 zsh 自動完成到搜尋比對工具鏈"
description: "紀錄一次跟 Claude Code 邊聊邊做的終端機環境優化過程，從 zsh 自動完成、指令記憶、語法上色，到補齊 fd／ast-grep／difftastic 這類搜尋比對工具，最後再回頭把常用唯讀指令整理進 Claude Code 的權限白名單。"
category:
- Tools
- Claude Code
tags: ["zsh", "Terminal", "Claude Code", "Homebrew", "Productivity", "Ghostty"]
---

# 痛點：每打一個指令，都要重新來一次

平常用 Claude Code 處理事情的時候，一直有兩個小摩擦一直在，只是還沒認真處理過。

第一個是 shell 本身太陽春：`~/.zshrc` 裡除了兩行 `PATH` 什麼都沒有，沒有自動完成、沒有指令記憶，翻歷史只能一路按上鍵慢慢找，找到類似指令還得手動改。第二個是跟 Claude Code 協作時，一些明明是唯讀、完全不會有副作用的指令（列檔案、查版本、curl 個 README），每次都要跳出來按一次允許，一來一回打斷節奏。

這篇記錄的就是一次把這兩件事一次盤點掉的過程：怎麼選工具、踩到什麼坑、以及最後怎麼跟 Claude Code 的權限系統對接起來。

---

# 解法一：先讓 zsh 自己記得你打過什麼

沒有裝 oh-my-zsh，也不想為了兩個功能扛一整套框架，所以直接挑最小夠用的兩個套件：

- **zsh-autosuggestions**：打字時用灰色文字提示之前打過的類似指令，按 `Ctrl+空白鍵` 或 `→` 接受
- **zsh-completions**：補強 tab 自動完成的涵蓋範圍

```bash
brew install zsh-autosuggestions zsh-completions
```

再搭配歷史紀錄設定，讓上下鍵可以依照目前已輸入的內容去篩選歷史，而不是整批往前翻：

```bash
# --- 指令歷史紀錄設定 ---
HISTFILE=~/.zsh_history
HISTSIZE=10000
SAVEHIST=10000
setopt SHARE_HISTORY       # 多個終端機視窗共用歷史紀錄
setopt HIST_IGNORE_DUPS
setopt HIST_IGNORE_ALL_DUPS
setopt HIST_FIND_NO_DUPS
setopt INC_APPEND_HISTORY  # 指令一輸入就馬上寫入歷史檔

autoload -Uz up-line-or-beginning-search down-line-or-beginning-search
zle -N up-line-or-beginning-search
zle -N down-line-or-beginning-search
bindkey "^[[A" up-line-or-beginning-search
bindkey "^[[B" down-line-or-beginning-search
```

裝完套件、加完設定，理論上重開終端機就能用了——實際上沒有這麼順利。

---

# 解法二：補顏色，讓終端機看得更快

自動完成裝好之後，順手把顏色也一起補了：

- **zsh-syntax-highlighting**：打指令時即時上色，有效指令綠色、無效指令紅色
- `ls -G`：資料夾、執行檔、連結各自不同顏色
- `grep --color=auto`：比對到的關鍵字直接標紅

```bash
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
alias grep='grep --color=auto'

source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

字體也一起處理：Ghostty 的設定檔（`~/Library/Application Support/com.mitchellh.ghostty/config.ghostty`）原本沒指定字體大小，吃系統預設的 13，直接加一行 `font-size = 16` 解決。

---

# 解法三：把 `ls`、`cat`、`cd` 也換成更聰明的版本

顏色跟自動完成算是基礎建設，接著補了三個常用指令的現代化替代品，選擇標準只有一個：**單一執行檔、沒有背景常駐程序，不會拖慢啟動速度**。

| 指令 | 替代品 | 換來什麼 |
|---|---|---|
| `ls` | `eza --icons` | 彩色、圖示、樹狀結構（`lt`） |
| `cat` | `bat --paging=never` | 語法高亮、行號 |
| `cd`（輔助） | `zoxide` | 記住常去的資料夾，`z proj` 直接跳過去 |

`fzf` 這次評估後沒有裝——目前的使用習慣還沒有到需要模糊搜尋歷史/檔案的程度，等真的有感覺卡再補。

---

# 解法四：讓 Claude Code 搜尋、比對檔案更快

前面幾項是「人用得爽」，這一項是「Claude Code 用得快」。Claude Code 內建的搜尋工具本身就是用 ripgrep（`rg`）在跑，`jq` 也已經裝了，所以缺的是這三個：

- **fd**：取代 `find`，語法簡單、速度快，列檔案清單特別有感
- **ast-grep**：**結構化**程式碼搜尋，不是純文字比對，而是看語法樹（AST）。可以搜「所有呼叫某函式且第一個參數是字串的地方」，對大範圍重構或精準搜尋特定寫法比 regex 準確很多
- **difftastic**（`difft`）：語法感知的 diff，函式搬位置也看得出來只是移動，不是整段被砍掉重寫

```bash
brew install fd ast-grep difftastic git-delta
```

`git-delta` 主要是給人看 `git diff` 用的，跟 Claude Code 本身無關，但反正裝了就順手一起弄。

---

# 解法五：把常用唯讀指令收進 Claude Code 的權限白名單

工具裝好之後浮出一個新問題：這些新指令第一次被 Claude Code 呼叫時，一樣得跳出來問一次要不要允許。用 `fewer-permission-prompts` 這個 skill 掃了最近幾個 session 的 transcript，統計出實際常跑、而且真的是唯讀的指令，整理出一份白名單：

```json
{
  "permissions": {
    "allow": [
      "Bash(curl -s https://raw.githubusercontent.com/*)",
      "Bash(curl -s \"https://api.github.com/*)",
      "Bash(brew list*)",
      "Bash(xcodes list*)",
      "Bash(xcodebuild -version)",
      "Bash(curl -sI *)",
      "Bash(difft *)",
      "Bash(delta *)"
    ]
  }
}
```

`fd`、`rg`、`jq` 沒有出現在清單裡，不是漏掉，是因為 Claude Code 本身已經把這幾個列為內建自動允許的唯讀指令，不需要再加規則。

---

# 容易忽略的三個坑

### 坑一：`compinit` 抱怨「insecure directories」

裝完套件重開終端機，第一次啟動就跳出：

```
zsh compinit: insecure directories, run compaudit for list.
```

用 `compaudit` 查出問題出在 `/opt/homebrew/share` 這層目錄權限太開放（group 有寫入權限）。`compinit` 在載入補全腳本前會檢查權限，只要有一層目錄是「別人也能寫」，就直接拒絕載入，避免有心人把惡意腳本塞進補全路徑裡執行。修法是 Homebrew 官方就建議的做法：

```bash
chmod go-w /opt/homebrew/share
chmod -R go-w /opt/homebrew/share/zsh
rm -f ~/.zcompdump*
```

清掉快取讓它重建一次，問題就沒再出現過。

### 坑二：`zsh-syntax-highlighting` 一定要放在檔案最後一行

這個套件的官方文件寫得很明白：這一行必須是 `.zshrc` 裡**最後被執行**的東西，放在 `zsh-autosuggestions` 或其他 `bindkey` 設定前面的話，語法上色跟自動建議會互相干擾、按鍵綁定也可能失效。整理設定檔的時候特別把它挪到檔案最尾端，而不是照裝的順序隨手加在中間。

### 坑三：`ast-grep` 沒有被放進白名單，是刻意的

白名單清單裡少了 `ast-grep`，不是漏掉。它預設跑起來是唯讀搜尋沒錯，但只要加上 `-U` / `--update-all` 參數就會直接改寫檔案。權限規則是前綴比對（prefix match），沒辦法只允許「不帶 `-U`」的用法——只要開了 `Bash(ast-grep *)` 這種大範圍規則，理論上就等於連改寫檔案的用法也一起放行了。

這跟專案原本對 `sed` 的處理邏輯是一致的：`sed` 也只有「唯讀運算式」的用法會被自動允許，帶有 in-place 編輯的用法一律還是要跳出來問一次。與其自己重新評估一次風險，不如直接沿用同一套判斷標準。

---

# 總結與效益

這次的終端機環境重整，說到底是把「人打字」跟「Claude Code 執行」兩件事分開優化：

1. **打字更少**：`zsh-autosuggestions` + 歷史篩選，重複指令幾乎不用重打
2. **看得更快**：語法上色、`eza`、`bat` 讓輸出一眼就能分辨重點
3. **找路更快**：`zoxide` 取代死記路徑，`fd` 取代龜速的 `find`
4. **Claude Code 搜得更準**：`ast-grep` 補上純文字比對做不到的結構化搜尋，`difftastic` 讓 diff 結果更貼近實際改動
5. **少按幾次允許**：把真正唯讀、且風險可控的指令收進白名單，剩下真正該問的（像 `ast-grep -U`）還是照樣會問

工具選擇的主軸自始至終沒變過：單一執行檔、沒有背景常駐、能不裝框架就不裝框架。真正花時間的反而是判斷「這個要不要放進白名單」——速度是其次，會不會不小心把一個能改檔案的指令包成看似安全的萬用規則，才是要謹慎的地方。
