---
layout: post
layout: post
title: "[Golang]gomobile 讓你在iOS/Android上面使用Golang(更新:11/17)"
description: ""
category: 
- golang
- ios的程式開發
tags: []

---



## 前言

其實[Go 1.5](https://golang.org/doc/go1.5)的正式公布後，最吸引人的就是與其他語言的相互合作．透過的可能是編譯成[c-shared library](https://golang.org/doc/go1.5#compiler_and_tools)．但是其實我個人最期待的，當然就是使用[go mobile](https://github.com/golang/mobile)．

最近他們把相關的範例跟工具都弄的差不多，並且有一了份相當[詳細的教學在這裡](https://github.com/golang/go/wiki/Mobile)．


### 簡單流程:

由於[教學](https://github.com/golang/go/wiki/Mobile)寫得很清楚，我這裡簡單的把事情都跑過一次．`注意：你需要有Go1.5`．


```

//抓取go mobile tool
go get golang.org/x/mobile/cmd/gomobile

//初始化gomobile，可能會花一點時間
gomobile init

//編譯gomobile裡面的範例
//這會在本地端直接編譯出 basic.app 只要透過xcode 複製到手機上即可(需要有開發者付費帳號)
gomobile build -target=ios golang.org/x/mobile/example/basic


//如果要把go package編譯成iOS可以用的framework
//這會在本地端編譯出一個hello.framework
cd $GOPATH/src/golang.org/x/mobile/example/bind
gomobile bind -target=ios golang.org/x/mobile/example/bind/hello

//直接在xcode拿來使用即可
open ios/bind.xcodeproj

```  

### 來個iOS簡單範例 (11/17更新)

這裡提供一個簡單的範例，主要也是測試一下幾個主要的疑慮:

簡單的範例程式碼在這裡，這裡有直接的[playgroud](http://play.golang.org/p/lrn7K9NiTs)

```go
package gomobile01

import (
	"fmt"
)

type Goomobile struct {
	Data string
}

//記得要有constructor 不然到時候你無法自行透過 NSObject init 來建立這個物件
func New() *Goomobile {
	return &Goomobile{Data: "Default test"}
}

func (g *Goomobile) GetData() string {
	fmt.Println("test go ")
	return g.Data
}
```

這時候建立這個iOS framework 指令是 (假設專案寫在kkdai/gomobile01)

```
$ gomobile bind -target=ios github.com/kkdai/gomobile01
```

然後會產生一個`gomobile01.framework` 把他拉進xcode專案． 以下提供如何使用的方式:

```objc

//載入該framework	
#import "gomobile01/Gomobile01.h"

```

這裡是使用的方式，切記要使用Constructor不要使用 `[[xx alloc] init]`

```objc
    GoGomobile01Goomobile *gb = GoGomobile01New();
    //Default data is @"Default test"
    [gb setData:@"Data...."];
    NSString * str = [gb GetData];
    //str = @"Data..."
```

#### 已知可以支援的部分

- 基礎的運算與簡單的資料格式當初參數 (`int`, `string`)
- 支援structure跟method
- 可以跑`net/http`但是記得別直接拿`rpc.Client`或是`http.Client`當作public


### 相關限制

目前試著編譯幾個有在使用的package發現有以下的問題，可能需要解決．


- `[]slice`, `map`, `interface{}` 比較進階的資料格式不支援 (可能也不能轉換到iOS)
    - 同理可證，一些package裡面的變數也不要放在parameter裡面．只支援基礎資料格式．
- 即便是簡單的`string`, `int`不支援public global variable．
    - `var TestVariable string` 不被允許
    - 但是`var testVariable string` 是可以允許的
- `const`目前還不支援 (gomobile: not yet supported, name for const)
- `structure need public:` 如果要使用某些物件，要能夠在Objective C上面使用就必須要記得改成public (big-case)
    - 這會發生問題`imported and not used: YOUR_PACKETGE`
    - 一般而言，public寫法是正確的，但是把struct改成private其實也不是不可以．
- `無法同時bind兩個package`: 由於裡面的記憶體處理或是其他處理的問題． 
    - 一般而言如果你跑了 `gomobile bind target=ios A_PACKAGE` 然後把a.framework拉近xcode在跑`gomobile bind target=ios B_PACKAGE`再把b.framework拉進專案，前面一個a.framework就會出現問題(不能跑)． 詳細的解釋在[這裡issue 12245](https://github.com/golang/go/issues/12245)．還沒有比較好的解法．
- [11/17] 如果要在Structure 必須要建立相關的constructor (`NewXXX()`)．
    
   

##參考文章

- [gomobileでiOSアプリをビルドする手順まとめ](http://golang.rdy.jp/2015/09/21/ios-gomobile/)
- [GoMobile Wiki](https://github.com/golang/go/wiki/Mobile)
- [關於Gomobile bind 的相關注意事項(官方)](https://godoc.org/golang.org/x/mobile/cmd/gobind)
- [Golang Talks for Gomobile "Go on Mobile"](https://talks.golang.org/2015/gophercon-go-on-mobile.slide)
