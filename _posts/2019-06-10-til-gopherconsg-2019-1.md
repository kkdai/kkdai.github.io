---
layout: post
title: "[研討會心得] GopherConSG 2019 (1)"
description: ""
category: 
- 研討會心得
- Golang
tags: ["研討會心得", "Golang", "GopherConSG", "Go"]
---




# 前提

偷個懶來看一下 GopherConSG 2019 ，順便了解幾個有趣的議題。 這邊分別提到了在 IDE 裡面，處理 autocomplete 的 gocode ，另一個議題則是由資安的角度，如何讓你的服務能夠更加的安全。

# Go, pls stop breaking my editor - GopherCon SG 2019

[Youtube]( https://www.youtube.com/watch?v=gZ7N3HulAb0)

<iframe width="560" height="315" src="https://www.youtube.com/embed/gZ7N3HulAb0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

看到很多人之前問的關於 gocode 的問題...
都表示 "Gocode 壞很久"，好像說 1.10 之後 Gocode 常常不會動。其實真的有些問題，而且分了不同 repo

- 1.10 nsf/gocode --> [mdempsky/gocode](mdempsky/gocode)
- 1.11 (**support go modules**) [mdempsky/gocode](mdempsky/gocode)--> [stamblerre/gocode](stamblerre/gocode)

回頭查 vscode-go https://github.com/microsoft/vscode-go/blob/9655d527548b286fee424983378560ce83adf8a2/src/goInstallTools.ts

預設使用 mdempsky/gocode

這個 session 就是 go team 認為他們應該花心思來維護一個讓每個編輯器都可以使用的  language server 

於是這就是 gopls (Go Language Server, pronounce "go please") 的由來

更多詳情可以看看影片



### Refer:

- https://github.com/mdempsky/gocode
- 

# Going Secure with Go - GopherCon SG 2019

[Youtube](https://www.youtube.com/watch?v=9e2gRtzemGo)

<iframe width="560" height="315" src="https://www.youtube.com/embed/9e2gRtzemGo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
講者從五個面向 ，來談談如何讓 Golang App 更有安全考量。

- **Data**:
  - 選擇好密碼的隨機法則
  - 確認 Kubernetes Secret 的安全性，其中講者也提到 Kubernetes secret 其實會存在 `/proc/<PID>/environ` ，其實相對不安全的。請愛用 [HashiCorp 的 Vault](https://www.hashicorp.com/products/vault/secrets-management)
- **Code**:
  - 需要常常做弱點掃描 [gosec](https://github.com/securego/gosec) 一定要用。
- **Dependency:**
  - 確認你使用的套件是跟上相關的修正，確認套件是否有任何安全性修復。
  - 很多時候，很多時候，小修正往往是修復安全性問題。
- **Container**:
  - 使用 Multi-stage build 來確保不會有任何安全疑慮
  - Policy (這邊指的是網路) ，要設定好並且有正確的考量。

整個 session 前顯易懂，並且有許多相關代碼的討論，很適合閱讀。