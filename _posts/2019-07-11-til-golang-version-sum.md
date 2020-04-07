---
layout: post
title: "[Golang] 快速整理一些 Go 版本上的差異（使用者角度） (Go version change note since Go 1.3 to 1.12)"
description: ""
category: 
- TodayILearn
- Golang
tags: ["Golang", "Go"]
---

![](https://golang.org/lib/godoc/images/go-logo-blue.svg)

## 前言：

其實從 Go 1.10 之後大概就沒有認真的在確認每個版本的變更之處，所以在這裡整理一下 Go 1.3 ~ 1.12 的變革。 一些快速懶人包如下：

- 幾個大變動：
  - Go 1.5 :  Pure Go for compiler and linker. go vendor.
  - Go 1.11: Go modules and WebAssembly support.

其他好奇的人可以查看 The State of Go.

(用英文寫，因為語言變化用中文實在不好表達)

## Go version history (major change):

Since Go 1.3, here only list few major changed you might be interested.



- **Go 1.3 (2014/06/18)** ([Detail Release note](https://golang.org/doc/go1.3))
	- Start 6 months release cycle.
	- Remove support for Windows 2000
	- Support DragonFly BSD, FreeBSD
	
- **Go 1.4 (2014/12/10)** ([Detail Release note](https://golang.org/doc/go1.4))

  - Support build ARM processor of Android system.
  - Support **[`go generate`](https://golang.org/cmd/go/#hdr-Generate_Go_files_by_processing_source)**
  - Speed is slightly faster than 1.3 
  
- *(Big change)* **Go 1.5 (2015/08/19)** ([Detail Release note](https://golang.org/doc/go1.5))

  - The compiler and runtime are now written entirely in Go (with a little assembler)
    - If you want to build go after 1.5, you need to install go 1.4.2 first.
  - Go programs run with `GOMAXPROCS` set to the number of cores available.
  -  [experimental support](https://golang.org/s/go15vendor) for "vendoring" 
  -  Add new tool `go tool trace`
  
- **Go1.6 ( 2016/02/17)**  ([Detail Release note](https://golang.org/doc/go1.6))
  - Experienmental port 64-bit MIPS
  - Experienmental port for Android x86 (32bit)
  - [HTTP/2 protocol](https://http2.github.io/) support
  - "GO15VENDOREXPERIMENT" default enable.
  
- **Go1.7 ( 2016/08/15)** ([Detail Release note](https://golang.org/doc/go1.7))
  - Support macOS 10.12 Sierra
  - Remove "GO15VENDOREXPERIMENT" support.
  - Full support  64-bit MIPS
  - Move `golang.org/x/net/context` into standard library `context`.
    - Add context support for [net](https://golang.org/doc/go1.7#net), [net/http](https://golang.org/doc/go1.7#net_http), and [os/exec](https://golang.org/doc/go1.7#os_exec).
  - [`net/http/httptrace`](https://golang.org/pkg/net/http/httptrace/) for tracing event in HTTP requests.
  
- **Go1.8 ( 2017/02/16)**  ([Detail Release note](https://golang.org/doc/go1.8))

  - Remove support for Mac OSX 10.7
  - `go fix` support context fix from  `golang.org/x/net/context` into standard library `context`. (which happen in 1.7) 
  -  [`plugin`](https://golang.org/pkg/plugin/) package for loading such plugins at run time. Plugin support is currently only available on Linux. 
  
- **Go1.9 (2017/08/24)**  ([Detail Release note](https://golang.org/doc/go1.9))
  - Go now supports type aliases, detail refer to [type alias design document](https://golang.org/design/18130-type-alias) and [an article on refactoring](https://talks.golang.org/2016/refactor.article).
  - Compiling a package's functions in parallel
  
- **Go1.10 (released 2018/02/16)**  ([Detail Release note](https://golang.org/doc/go1.10))
  - For travis CI, you need to specific version in "1.10", rather than 1.10.
  - No need set for `GOROOT` and `GOTMPDIR`.
  - The `go test` command now caches test results, to bypass test caching using `go test -count=1` to verify if any test failure unexpectly.
  
- *(Big change)* **Go1.11 ( 2018/08/24)**  ([Detail Release note](https://golang.org/doc/go1.11))

  - Go 1.11 requires:
    -  OS X 10.10 Yosemite or later.
    - Windows 7 or later.
  - Go 1.11 adds an experimental port to [WebAssembly](https://webassembly.org/)
  - Go 1.11 adds preliminary support for a [new concept called “modules,”](https://golang.org/cmd/go/#hdr-Modules__module_versions__and_more), `GO111MODULE` default off.

- **Go1.12 ( 2019/02/25)**  ([Detail Release note](https://golang.org/doc/go1.12))
  - Go's new `windows/arm` port supports running Go on Windows 10 IoT Core on 32-bit ARM chips such as the Raspberry Pi 3.
	- `GO111MODULE` default is on, (go modules will default enable)
  - `go tool vet` no longer supported. 
	- `godoc` and `go doc`:
    - `go doc` begin from 1.4, compatible with `godoc`
    - `godoc` goes to pure web service since 1.12.
    - `go doc` goes to command line tool since 1.12.
    - The [`ReverseProxy`](https://golang.org/pkg/net/http/httputil/#ReverseProxy) now automatically proxies WebSocket requests.



## **Reference:**

- [The State of Go](https://pocketgophers.com/state-of-go/)
- [Go Release History](https://golang.org/doc/devel/release.html)