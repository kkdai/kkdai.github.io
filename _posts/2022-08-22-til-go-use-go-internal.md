---
layout: post
title: "[學習文件][Golang] 從 Go 原始碼裡面拆解出的好用套件 go-internal "
description: ""
category: 
- 學習文件
tags: ["Golang", "test-tool"]
---

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Released a new version of <a href="https://twitter.com/rogpeppe?ref_src=twsrc%5Etfw">@rogpeppe</a>&#39;s go-internal with some notable changes to testscript, used heavily in <a href="https://twitter.com/cue_lang?ref_src=twsrc%5Etfw">@cue_lang</a> :) A special thanks to <a href="https://twitter.com/bitfield?ref_src=twsrc%5Etfw">@bitfield</a>, <a href="https://twitter.com/tomwpayne?ref_src=twsrc%5Etfw">@tomwpayne</a>, and <a href="https://twitter.com/FiloSottile?ref_src=twsrc%5Etfw">@FiloSottile</a> for their recent contributions! <a href="https://twitter.com/hashtag/golang?src=hash&amp;ref_src=twsrc%5Etfw">#golang</a><a href="https://t.co/hDAqsecssu">https://t.co/hDAqsecssu</a></p>&mdash; Daniel Martí (@mvdan_) <a href="https://twitter.com/mvdan_/status/1561967605073723392?ref_src=twsrc%5Etfw">August 23, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

有一天這樣的 [tweet](https://twitter.com/mvdan_/status/1561967605073723392) 吸引到我，加上裡面有提到

- 前Google 資安大神  [@FiloSottile](https://twitter.com/FiloSottile) 
- Power of Go 作者 [@bitfield](https://twitter.com/bitfield)

讓我認真的想去了解一下什麼是 [Go-Internal](https://github.com/rogpeppe/go-internal)  。

## 什麼是 Go-Internal 

#### 位置： [https://github.com/rogpeppe/go-internal]( https://github.com/rogpeppe/go-internal)

不知道有沒有人跟我一樣，有時候在看 golang source code 的時候，會看到一些工具或是 func 相當好用。 當下可能會複製拿出來使用。或是擷取某些精髓。 但是很多時候，有很多 func 整理過後往往可以成為很方便的工具。

而 [https://github.com/rogpeppe/go-internal](https://github.com/rogpeppe/go-internal) 就是一個將 Go Internal source code 重新整理後，獨立出來使用的小套件。其實裡面有許多很好用的小工具可以使用。

以下挑選幾個小工具，稍微分享一下：

### 關於 go-Internal/testscript 套件

**testscript**: script-based testing based on txtar files

這個工具提供了讓你測試 cli command app 的測試包。這樣看可能還不知道，裡面的 `testscript` 要怎麼使用。但是從 [Reddit](https://www.reddit.com/r/golang/comments/c67zv0/unit_testing_cobra/) 裡面的介紹蠻明白的。

```
testscript was factored out of the cmd/go internals where it is used to test the go command itself in various combinations/permutations.
```

也可以參考一段 code 來自 [flowdev](https://github.com/flowdev)/**[spaghetti-analyzer](https://github.com/flowdev/spaghetti-analyzer)** [main_test.go](https://github.com/flowdev/spaghetti-analyzer/blob/2c8b0a97c4c1c24190ae221ff41323d5d53642b1/main_test.go)

```
func TestAnalyze(t *testing.T) {
	testscript.Run(t, testscript.Params{
		Dir: "testdata",
		Cmds: map[string]func(*testscript.TestScript, bool, []string){
			"analyze": func(ts *testscript.TestScript, _ bool, args []string) {
				expectedReturnCode, err := strconv.Atoi(args[0])
				if err != nil {
					ts.Fatalf("fatal return code error (%q): %v", args[0], err)
				}

				args = args[1:]
				actualReturnCode := analyze(args)

				if actualReturnCode != expectedReturnCode {
					ts.Fatalf("Expected return code %d but got: %d", expectedReturnCode, actualReturnCode)
				}
			},
		},
		TestWork: false,
	})
}
```

這一段用就是讀取 `testdata` 資料夾的檔案，然後跑裡面的 cli test case 來跑你的 Cli app 。

### 關於 go-Internal/testenv 套件

這個套件蠻有趣的，可以找出目前系統 golang 相關環境參數。 比如說，能夠執行 `go run`  環境。

```
package main

import (
	"fmt"

	"github.com/rogpeppe/go-internal/testenv"
)

func main() {
	fmt.Println(testenv.HasGoRun())
}

```

這一段 code 可以檢查你的 docker image 有沒有 `go run` 環境。  詳細 testenv source code 可以看[這裡](https://github.com/rogpeppe/go-internal/blob/master/testenv/testenv.go)。

### 關於 go-Internal/par 套件

*Package par implements parallel execution helpers.*  可以讓你跑許多 *parallel* 的小幫手。 

```
import (
	"fmt"
	"sync/atomic"
	"time"

	"github.com/rogpeppe/go-internal/par"
)

func main() {
	var w par.Work
	const N = 100
	for i := 0; i < N; i++ {
		w.Add(i)
	}
	start := time.Now()
	var n int32
	w.Do(N, func(x interface{}) {
		time.Sleep(1 * time.Millisecond)
		fmt.Println(n)
		atomic.AddInt32(&n, +1)
	})
	if n != N {
		fmt.Println("par.Work.Do did not do all the work")
	}
	if time.Since(start) < N/2*time.Millisecond {
		fmt.Println("done!")
		return
	}
}

```

這樣就可以有 1 ~ 100 個不同 *parallel* 同時在跑。

## 小結論：

偶然間看 tweet 既然可以看到這樣好用的小工具，重點還是幾位大神一起維護的。 有興趣的可以持續關注。

## 相關文章：

- [Reddit: Unit Testing Cobra](https://www.reddit.com/r/golang/comments/c67zv0/unit_testing_cobra/)
- [Tweet about go-internal 1.9 release](https://twitter.com/mvdan_/status/1561967605073723392)
