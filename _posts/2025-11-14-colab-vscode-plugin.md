---
layout: post
title: "[VS Code][Colab] Google 正式釋出 Colab vS Code Plugin "
description: ""
category:
- TIL
tags: ["Python", "Colab", "Gemini"]

---



![Connecting to a new Colab server and executing a code cell](https://github.com/googlecolab/colab-vscode/raw/HEAD/docs/assets/hello-world.gif)



# 前情提要

[Google Colab](https://colab.research.google.com/) 是一個我很喜歡的服務，你可以在線上透過 JupyterNotebook 的介面，快速使用到 GPU (甚至是 TPU)。有許多需要大量運算資源的東西，都可以很快速的在遠端的機器上面執行。

我自己很常在上面去嘗試一些模型，雖然常常排隊排不到機器。



<img src="../images/image-20251114155345861.png" alt="image-20251114155345861" style="zoom:25%;" />



## 使用 Colab 可能有的痛點

雖然使用[Google Colab](https://colab.research.google.com/)  機器非常的方便，但是由於在線上編輯有一些比較麻煩的地方：

- 無法使用 Copilot 這類型的 Code Assist Tool 來幫我 Auto-Complete 一些程式碼
- 無法跑 Gemini CLI Code Assist 來幫我寫出一些更多的測試或是幫忙想應用。



## Colab for VS Code Plugin

但是現在 [Colab VS Code Extension](https://marketplace.visualstudio.com/items?itemName=Google.colab) 終於可以在 VS Code Plugin 上面使用了。你可以透過 “Colab" 直接找到官方釋出的 Plugin 。

<img src="../images/Code 2025-11-14 15.55.03.png" alt="Code 2025-11-14 15.55.03" style="zoom:50%;" />

安裝過程相當的簡單又快速。



## 連線到 Colab 

<img src="../images/Code 2025-11-14 15.23.10.png" alt="Code 2025-11-14 15.23.10" style="zoom:25%;" />

如果要連線，在選擇 Kernel 的時候，就可以選擇 Colab 來遠端連線。

<img src="../images/Code 2025-11-14 15.23.16.png" alt="Code 2025-11-14 15.23.16" style="zoom:25%;" />

這裡還可以快速連線，或是找你上次連線過的伺服器。



<img src="../images/Code 2025-11-14 15.23.22.png" alt="Code 2025-11-14 15.23.22" style="zoom:25%;" />

這裡就是讓人興奮的地方，可以找找 TPU （不保證排得到隊伍）來用用看。

這樣就可以了。



## 實際應用：

![Code 2025-11-14 15.54.52](../images/Code 2025-11-14 15.54.52.png)

這樣比較對味啦！！ Vibe Coding 出現之後，我們越來越習慣 Vibe Coding 了。但是如果需要 Step by Step 的去偵錯，或是想要跑一些大型機器才能運行的運算。真的還是需要透過 Colab 來幫忙，但是如果又希望可以有 Gemini CLI 的輔助的話，或許  [Colab VS Code Extension](https://marketplace.visualstudio.com/items?itemName=Google.colab)  就是你不可或缺的好夥伴。



## 目前一些需要注意的地方

