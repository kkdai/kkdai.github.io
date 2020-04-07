---
layout: post
title: "[TIL] 關於 Golang 1.11 之後在 VSCODE 裡面的 GOROOT 設定"
description: ""
category: 
- TodayILearn
- vscode
- Golang
tags: ["TIL", "vscode", "GitHub"]
---



# 關於 vscode-go 的設定

使用 vscode 並且使用 Golang 1.11 之後版本的人，每次 Golang 版本更新可能會出現。

`go: cannot find GOROOT directory: /usr/local/Cellar/go/1.12.1/libexec`

之前我都手動改 vscode 裡面 goroot 設定，但是其實只要在 `settings.json` 增加以下這行就好。

```
{
....
"go.inferGopath":true,
....
}
```



# Reference

- https://github.com/Microsoft/vscode-go/issues/1879
- https://github.com/Microsoft/vscode-go/wiki/GOPATH-in-the-VS-Code-Go-extension