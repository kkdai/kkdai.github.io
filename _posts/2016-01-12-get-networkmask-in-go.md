---
layout: post
layout: post
title: "[Go]取得實體網卡的IPv4網路遮罩(Get device network mask)"
description: ""
category: 
- golang
tags: [go]
---


![](http://synflood.at/tmp/golang-slides/images/gophercolor.png)

##前言

最近在研究一些Golang在UDP上的處理，主要是要抓取網路遮罩(network mask)做一些進階使用．就發現要取得正確的網路遮罩，其實是蠻困難的．由於是無法直接使用到裝置上面的網卡設定．

這邊介紹一下相關的流程:

###先來抓取IPv4

這邊先顯示抓取IP的方式:

```go
func GetIP() net.IP {

	ifaces, err := net.Interfaces()
	// handle err
	if err != nil {
		log.Println("No network:", err)
		return nil
	}

	for _, i := range ifaces {
		//只抓取網路卡名稱為"en0", "en1"...
		if strings.Contains(i.Name, "en") {
			addrs, err := i.Addrs()
			// handle err
			if err != nil {
				log.Println("No IP:", err)
				return nil
			}

			for _, addr := range addrs {
				var ip net.IP
				switch v := addr.(type) {
				case *net.IPNet:
					log.Println("IPNET")
					ip = v.IP
				case *net.IPAddr:
					log.Println("IPAddr")
					ip = v.IP
				}
				
				//這裡會抓取兩種IP，分別是IPv4與IPv6
				if ip[0] == 0 {
					//第一個byte是0為IPv4
					log.Println("Get device:", i.Name)
					return ip
				}
			}
		}
	}

	return nil
}
```

透過這個方式可以列舉所有的網路卡並且把第一張具有IPv4資料的`net.IP`傳回來．

###再來找Network Mask

其實透過官方的API，原本就有一個`IP.DefaultMask()`的[函式](https://golang.org/pkg/net/#IP.DefaultMask)可以使用．不過抓來的**Network Mask是透過計算而來的，而不是跟網卡拿的**．透過以下的範例:


```go
 package main

 import (
         "fmt"
         "net"
 )

 func main() {
         addr := net.ParseIP("192.168.1.1")
         mask := addr.DefaultMask()
         fmt.Printf("Address : %s \n Network : %s \n", addr.String(), mask)
        //Mask 255, 255, 255, 0
 
         addr = net.ParseIP("172.16.110.123")
         mask = addr.DefaultMask()
         fmt.Printf("Address : %s \n Network : %s \n", addr.String(), mask)
        //Mask 255, 255, 0, 0
}
```

這邊可以看到，如果你的內部網路不是設定`192.168.XXX.XXX`的話，你抓到的mask都會是`255.255.0.9`．這樣的結果跟你在網路卡上面查到的可能會完全不相同．


###跟網卡拿取資料

比較好也比較不容易有問題的方式，就是直接透過系統的指令(console)去跟OS拿取網卡的資料．這裡貼上兩種方式:


#### Windows 版本(目前Go 1.5.2 broken，預計Go 1.6會修好)

網路上搜尋到的應該都是透過這個方式，不過在Go 1.5.2似乎broken了，聽說Go 1.6會修好． 可以參考這個[Go: issue 12551](https://github.com/golang/go/issues/12551)


<script src="https://gist.github.com/kkdai/b27461fa646212395bcc.js"></script>

#### Mac OSX的版本，透過Ipconfig指令來抓取

這是我自己尋找的，透過Mac OS X的命令列來抓取資料，其實是比較簡單也比較方便的．不會因為版本有太大的問題． 使用方式就是call `GetNetMask("en0")`


<script src="https://gist.github.com/kkdai/fa51609f40036cee4ec9.js"></script>



###結論:

要抓取IP可以透過`net` package來抓取，但是需要IPv4的網路遮罩，還是得要跟實體設備直接抓取比較妥當． 不過之後都要使用IPv6了，就不用那麼麻煩啦．  

## 參考練結:

- [Stackoverflow: How to get all addresses and masks from local interfaces in go?](http://stackoverflow.com/questions/23529663/how-to-get-all-addresses-and-masks-from-local-interfaces-in-go)
- [GoDoc: net](https://golang.org/pkg/net/)
- [Go: issue 12551](https://github.com/golang/go/issues/12551)
