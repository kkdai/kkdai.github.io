---
layout: post
title: "[TIL] 關於 Docker Remote API 的學習"
description: ""
category: 
- TodayILearn
tags: ["docker", "TIL"]

---

![](https://docs.docker.com/engine/article-img/architecture.svg)

## 什麼是 Docker Remote API

每個 Docker Daemon 其實都有提供 [Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api/) ．其工作主要都是提供一個介面可以讓 Docker Client 透過 API 來控制．  舉凡大家日常使用的指令 `docker images` 或是 `docker ps` 其實都是使用 Docker Remote API 的方式來連接本地端的 Daemon 來對 Docker Daemon Service 下指令．

此外， `docker-machine` 對於遠端的機器也是透過 Docker Remote API 的方式．


![](https://docs.docker.com/engine/reference/api/images/event_state.png)

## 如何在 Mac OSX for Docker 使用 Docker Remote API

### 尋找 Docker-Machine Certificate

當你使用 `Docker-Machine` 來建置機器的時候，其實你的 Certificate 會存放在 `/Users/YOUR_USERNAME/.docker/machine/machines` 裡面．

該 Certificate 就是存放著讓 Docker Client 與 Docker Daemon 能夠相互溝通的金鑰資訊．

#### 舉個例子:

如果你有建立一個 Docker-Machine 的機器名稱為 `default`，那麼 Certificate 檔案就會存放在: 

`/Users/YOUR_USERNAME/.docker/machine/machines/default`

裡面．

### 透過 `curl` 連接 Docker Remote API

以我的名稱 `Evan` 的作為一個範例的話：

```
### Set your environment variable 
> export DOCKER_CERT_PATH=/Users/Evan/.docker/machine/machines/default
 

### Get Docker Machine IP
> docker-machine ip default
192.168.99.100

## Use CURL
> curl https://192.168.99.100:2376/images/json --cert $DOCKER_CERT_PATH/cert.pem --key $DOCKER_CERT_PATH/key.pem --cacert $DOCKER_CERT_PATH/ca.pem

curl: (58) SSL: Can't load the certificate "/Users/Evan/.docker/machine/machines/default/cert.pem" and its private key: OSStatus -25299
```

如果出現以下的錯誤:
```
curl: (58) SSL: Can't load the certificate "/Users/Evan/.docker/machine/machines/default/cert.pem" and its private key: OSStatus -25299
```

代表你沒有 `cert.pem` 需要執行額外的 key 轉換的動作

```
> cd /Users/Evan/.docker/machine/machines/default

# key transfer
> openssl pkcs12 -export \
-inkey key.pem \
-in cert.pem \
-CAfile ca.pem \
-chain \
-name client-side \
-out cert.p12 \
-password pass:mypass
```

接下來再來執行:

```
> curl https://192.168.99.100:2376/images/json --cert $DOCKER_CERT_PATH/cert.p12 --pass mypass --key $DOCKER_CERT_PATH/key.pem --cacert $DOCKER_CERT_PATH/ca.pem

[
  {
    "Id": "sha256:d38beda529d3274636d6cb1c9000afe4f00fbdcfa544140d6cc0f5d7f5b8434a",
    "ParentId": "",
    "RepoTags": [
      "arungupta/couchbase:latest"
    ],
    "RepoDigests": null,
    "Created": 1450330075,
    "Size": 374824677,
    "VirtualSize": 374824677,
    "Labels": {
      
    }
  }
]
```
就會回傳 Docker Machine 中的 Docker Daemon 的狀態．

### 查詢 Docker Container 的執行狀態，使用率

`docker stats` 是一個很方便的指令，可以顯示出目前在執行的 Container  的狀態與使用率．

這邊舉出一個簡單的例子：

```
>docker stats

CONTAINER           CPU %               MEM USAGE / LIMIT       MEM %               NET I/O               BLOCK I/O           PIDS
fb5435a83a96        0.00%               48.76 MiB / 1.954 GiB   2.44%               117.5 kB / 76.08 kB   19.86 MB / 0 B      9
```

### 透過 Docker Remote API 查詢 Docker stats

如果前面的設定都已經設定好了，你也可以透過 Docker Remote API 來查詢遠端機器

```
## 假設在 docker-machine default 裡面跑有兩個 container
> docker-machine ssh default

> docker ps

CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS               NAMES
66ee7abb2a12        instavote/vote:latest   "gunicorn app:app -b "   5 days ago          Up 5 days           80/tcp              voteV.1.3u2i8gnttseibckfsnqr9dp58
e510fa405940        instavote/vote:movies   "gunicorn app:app -b "   5 days ago          Up 5 days           80/tcp              vote.1.3hozslyep7al27u35zb24o6mp

## 查詢該 Container 66ee7abb2a12 的狀態
> curl https://192.168.99.100:2376/containers/66ee7abb2a12/stats --cert $DOCKER_CERT_PATH/cert2.p12 --pass mypass --key $DOCKER_CERT_PATH/key.pem --cacert $DOCKER_CERT_PATH/ca.pem

{
  "read": "2016-08-15T05:10:49.404713534Z",
  "precpu_stats": {
    "cpu_usage": {
      "total_usage": 0,
      "percpu_usage": null,
      "usage_in_kernelmode": 0,
      "usage_in_usermode": 0
    },
    "system_cpu_usage": 0,
    "throttling_data": {
      "periods": 0,
      "throttled_periods": 0,
      "throttled_time": 0
    }
  },
  "cpu_stats": {
    "cpu_usage": {
      "total_usage": 26505881978,
      "percpu_usage": [
        26505881978
      ],
      "usage_in_kernelmode": 1690000000,
      "usage_in_usermode": 5730000000
    },
    "system_cpu_usage": 110834160000000,
    "throttling_data": {
      "periods": 0,
      "throttled_periods": 0,
      "throttled_time": 0
    }
  },
  "memory_stats": {
    "usage": 56270848,
    "max_usage": 75644928,
    "stats": {
      "active_anon": 55996416,
      "active_file": 274432,
      "cache": 286720,
      "dirty": 4096,
      "hierarchical_memory_limit": 9223372036854771712,
      "hierarchical_memsw_limit": 9223372036854771712,
      "inactive_anon": 0,
      "inactive_file": 0,
      "mapped_file": 0,
      "pgfault": 363243,
      "pgmajfault": 0,
      "pgpgin": 318130,
      "pgpgout": 306436,
      "rss": 55984128,
      "rss_huge": 8388608,
      "swap": 0,
      "total_active_anon": 55996416,
      "total_active_file": 274432,
      "total_cache": 286720,
      "total_dirty": 4096,
      "total_inactive_anon": 0,
      "total_inactive_file": 0,
      "total_mapped_file": 0,
      "total_pgfault": 363243,
      "total_pgmajfault": 0,
      "total_pgpgin": 318130,
      "total_pgpgout": 306436,
      "total_rss": 55984128,
      "total_rss_huge": 8388608,
      "total_swap": 0,
      "total_unevictable": 0,
      "total_writeback": 0,
      "unevictable": 0,
      "writeback": 0
    },
    "failcnt": 0,
    "limit": 1044246528
  },
  "blkio_stats": {
    "io_service_bytes_recursive": [
      
    ],
    "io_serviced_recursive": [
      
    ],
    "io_queue_recursive": [
      
    ],
    "io_service_time_recursive": [
      
    ],
    "io_wait_time_recursive": [
      
    ],
    "io_merged_recursive": [
      
    ],
    "io_time_recursive": [
      
    ],
    "sectors_recursive": [
      
    ]
  },
  "pids_stats": {
    "current": 5
  },
  "networks": {
    "eth0": {
      "rx_bytes": 648,
      "rx_packets": 8,
      "rx_errors": 0,
      "rx_dropped": 0,
      "tx_bytes": 648,
      "tx_packets": 8,
      "tx_errors": 0,
      "tx_dropped": 0
    },
    "eth1": {
      "rx_bytes": 648,
      "rx_packets": 8,
      "rx_errors": 0,
      "rx_dropped": 0,
      "tx_bytes": 648,
      "tx_packets": 8,
      "tx_errors": 0,
      "tx_dropped": 0
    }
  }
}
```

## 透過 Golang Code 來直接查詢

Docker 官方有提供一個很方便的 Golang 套件 [engine-api](https://github.com/docker/engine-api)

 如果想要找到其他語言的套件，可以到這個地方去找 [Docker Remote API client libraries](https://docs.docker.com/engine/reference/api/remote_api_client_libraries/)．
 
 

```
package main

import (
	"fmt"
	"io/ioutil"
	"log"

	"github.com/docker/engine-api/client"
	"github.com/docker/engine-api/types"
	"golang.org/x/net/context"
)

func main() {
	defaultHeaders := map[string]string{"User-Agent": "engine-api-cli-1.0"}
	cli, err := client.NewClient("unix:///var/run/docker.sock", "v1.22", nil, defaultHeaders)
	if err != nil {
		panic(err)
	}

	options := types.ContainerListOptions{All: true}
	containers, err := cli.ContainerList(context.Background(), options)
	if err != nil {
		panic(err)
	}

	for _, c := range containers {
		fmt.Println(c.ID)
		body, err := cli.ContainerStats(context.Background(), c.ID, false)
		defer body.Close()
		if err != nil {
			log.Println(err)
		}
		
		//顯示 Container 資訊
		content, err := ioutil.ReadAll(body)
		log.Println("Container:", c.ID, string(content))
	}
}
```

透過這樣查詢，可以得到類似以下的資訊:

```
2016/08/15 15:57:55 Container: fb5435a83a96a753e61b3020d6478650158ae9382f41f3119e3feafb65d2e07d

{
  "read": "2016-08-15T07:57:55.652878954Z",
  "precpu_stats": {
    "cpu_usage": {
      "total_usage": 709001154,
      "percpu_usage": [
        428133113,
        31262531,
        45274798,
        204330712
      ],
      "usage_in_kernelmode": 50000000,
      "usage_in_usermode": 590000000
    },
    "system_cpu_usage": 185639070000000,
    "throttling_data": {
      "periods": 0,
      "throttled_periods": 0,
      "throttled_time": 0
    }
  },
  "cpu_stats": {
    "cpu_usage": {
      "total_usage": 709001154,
      "percpu_usage": [
        428133113,
        31262531,
        45274798,
        204330712
      ],
      "usage_in_kernelmode": 50000000,
      "usage_in_usermode": 590000000
    },
    "system_cpu_usage": 185643040000000,
    "throttling_data": {
      "periods": 0,
      "throttled_periods": 0,
      "throttled_time": 0
    }
  },
  "memory_stats": {
    "usage": 51126272,
    "max_usage": 60538880,
    "stats": {
      "active_anon": 30945280,
      "active_file": 1474560,
      "cache": 19976192,
      "dirty": 0,
      "hierarchical_memory_limit": 9223372036854771712,
      "hierarchical_memsw_limit": 9223372036854771712,
      "inactive_anon": 36864,
      "inactive_file": 18386944,
      "mapped_file": 15933440,
      "pgfault": 12593,
      "pgmajfault": 153,
      "pgpgin": 16899,
      "pgpgout": 6530,
      "rss": 30867456,
      "rss_huge": 4194304,
      "swap": 0,
      "total_active_anon": 30945280,
      "total_active_file": 1474560,
      "total_cache": 19976192,
      "total_dirty": 0,
      "total_inactive_anon": 36864,
      "total_inactive_file": 18386944,
      "total_mapped_file": 15933440,
      "total_pgfault": 12593,
      "total_pgmajfault": 153,
      "total_pgpgin": 16899,
      "total_pgpgout": 6530,
      "total_rss": 30867456,
      "total_rss_huge": 4194304,
      "total_swap": 0,
      "total_unevictable": 0,
      "total_writeback": 0,
      "unevictable": 0,
      "writeback": 0
    },
    "failcnt": 0,
    "limit": 2097557504
  },
  "blkio_stats": {
    "io_service_bytes_recursive": [
      {
        "major": 254,
        "minor": 0,
        "op": "Read",
        "value": 19857408
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Write",
        "value": 0
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Sync",
        "value": 0
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Async",
        "value": 19857408
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Total",
        "value": 19857408
      }
    ],
    "io_serviced_recursive": [
      {
        "major": 254,
        "minor": 0,
        "op": "Read",
        "value": 559
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Write",
        "value": 0
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Sync",
        "value": 0
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Async",
        "value": 559
      },
      {
        "major": 254,
        "minor": 0,
        "op": "Total",
        "value": 559
      }
    ],
    "io_queue_recursive": [
      
    ],
    "io_service_time_recursive": [
      
    ],
    "io_wait_time_recursive": [
      
    ],
    "io_merged_recursive": [
      
    ],
    "io_time_recursive": [
      
    ],
    "sectors_recursive": [
      
    ]
  },
  "pids_stats": {
    "current": 9
  },
  "networks": {
    "eth0": {
      "rx_bytes": 117457,
      "rx_packets": 393,
      "rx_errors": 0,
      "rx_dropped": 0,
      "tx_bytes": 76077,
      "tx_packets": 261,
      "tx_errors": 0,
      "tx_dropped": 0
    }
  }
}
```

## 參考鏈結

- [Monitoring Docker Containers - docker stats, cAdvisor, Universal Control Plane](http://blog.couchbase.com/2016/april/monitoring-docker-containers-docker-stats-cadvisor-universal-control-plane)
- [Docker Remote API](https://docs.docker.com/engine/reference/api/docker_remote_api/)
- [Enabling Docker Remote API on Docker Machine on Mac OS X](http://blog.couchbase.com/2016/february/enabling-docker-remote-api-docker-machine-mac-osx)
- [Docker Remote API client libraries](https://docs.docker.com/engine/reference/api/remote_api_client_libraries/)