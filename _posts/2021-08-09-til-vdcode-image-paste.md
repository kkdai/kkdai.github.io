---
layout: post
title: "[TIL] 透過 VSCode 寫 Markdown 貼圖的利器 - Markdown Paste"
description: ""
category: 
- TodayILearn
tags: ["vscode", "Tools"]
---


![](https://github.com/telesoho/vscode-markdown-paste-image/raw/master/res/markdown_paste_demo_min.gif)


## VSCode Plugin - Markdown Paste :

*完整 Github*: [https://github.com/telesoho/vscode-markdown-paste-image](https://github.com/telesoho/vscode-markdown-paste-image)

你是用哪一套工具寫部落格？ 之前喜歡使用 Typora (這邊可以看我寫的[文章](https://www.evanlin.com/til-mdeditor-typora/)) 。 寫起來雖然很快，但是由於使用 github page 加上後台是 Jekyll 的原因，貼圖的時候總是相當的不方便。

最在查詢筆記軟體的時候，看到了這個 VSCode 的 [Plugin - Markdown Paste](https://marketplace.visualstudio.com/items?itemName=telesoho.vscode-markdown-paste-image) ，覺得相當好用。

有幾個大家一定會寫歡的功能：

### 直接貼上剪貼簿圖片

快速鍵：(MacOSX: Command + Option + V )

![](../images/2021/2021-08-10-16-56-16.png)

這個功能相當好用（主要也是為了這個功能來用的）。有一個比較麻煩的是，下載的圖片位置預設會放在 `./` 這邊可以稍微修改，透過修改設定就可以。

![](../images/2021/2021-08-10-17-01-28.png)

進去去直接修改 `MarkdownPaste.path`，以我的 case 就是改成 `./../images/2021`。這樣一年改一次就好，圖片也可以保留在我的 Github 上面，不怕找不到。

上面這張圖就是透過剪貼簿，馬上貼上。真的很方便，也不用管命名問題。（你之到 RD 很怕叫他取名字，檔案名稱也是一樣 XD)。

其實還有很多功能，大家可以到 Marketplace 網頁去查看。我也會持續研究，慢慢看有哪些習慣的功能。



## Typora 沒有嗎？

<img src="../images/2021/image-20210810214250200.png" width="500px">

(更新 2021/08/10) 貼完臉書後，看到社群大大們的建議，發現好像 Typora 今年也有相關的更新了。其實今年(2021/02)的版本更新 (0.9.9.32) 更新的 ([Typora 更新](https://support.typora.io/What's-New-0.9.84/))。但是實際使用後，發現需要相關的設定如下。

<img src="../images/2021/image-20210810214602485.png" width="600px" >

- 需要修改複製圖片到「自訂資料夾」(注意原本的變數要移除)

- 因為檔案要放到 github page ，記得要優先使用相對路徑。

  

## 相關文章：

- [[TIL][markdown] 好用的編輯器 - typora](https://www.evanlin.com/til-mdeditor-typora/)
- [VSCode Marketplace: Markdown Paste](https://marketplace.visualstudio.com/items?itemName=telesoho.vscode-markdown-paste-image)
- [Typora Image Upload](https://support.typora.io/What's-New-0.9.84/)
