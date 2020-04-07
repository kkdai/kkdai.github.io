---
layout: post
title: "[TIL] Migrate solidity from remix to truffle (1) -  test "
description: ""
category: 
- TodayILearn
- vscode
tags: ["TIL", "blockchain", "solidity", "truffle"]
---



![](../images/2019/truffle.png)



# 前提





# 內容分享

關於 Comipile migration 部分請找[[TIL] Migrate solidity from remix to truffle (1) - Compile](http://www.evanlin.com/til-remix-truffle/)





# 總結:



### 環境沒起來

```

Could not connect to your Ethereum client with the following parameters:
    - host       > localhost
    - port       > 9545
    - network_id > *
Please check that your Ethereum client:
    - is running
    - is accepting RPC connections (i.e., "--rpc" option is used in geth)
    - is accessible over the network
    - is properly configur
```

經過查詢過後發現因為沒有把 `geth` 安裝起來，透過以下方式可以安裝:

- 到 <https://github.com/ethereum/go-ethereum> clone 並且 make
- 或是直接透過 `brew` 來安裝， `brew install gth`

安裝過後，直接執行 `geth —rpc` 然後查詢你的 go-ethereum client 所使用的 port 。

```
  ~/ geth --rpc                                                                    /default ⎈
INFO [04-28|07:45:36.381] Maximum peer count                       ETH=25 LES=0 total=25
INFO [04-28|07:45:36.402] Starting peer-to-peer node               instance=Geth/v1.8.27-stable/darwin-amd64/go1.12.4
INFO [04-28|07:45:36.402] Allocated cache and file handles         database=/Users/evan/Library/Ethereum/geth/chaindata cache=512 handles=5120
INFO [04-28|07:45:36.432] Writing default main-net genesis block
INFO [04-28|07:45:36.751] Persisted trie from memory database      nodes=12356 size=1.88mB time=61.15903ms gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [04-28|07:45:36.751] Initialised chain configuration          config="{ChainID: 1 Homestead: 1150000 DAO: 1920000 DAOSupport: true EIP150: 2463000 EIP155: 2675000 EIP158: 2675000 Byzantium: 4370000 Constantinople: 7280000  ConstantinopleFix: 7280000 Engine: ethash}"
INFO [04-28|07:45:36.751] Disk storage enabled for ethash caches   dir=/Users/evan/Library/Ethereum/geth/ethash count=3
INFO [04-28|07:45:36.752] Disk storage enabled for ethash DAGs     dir=/Users/evan/.ethash                      count=2
INFO [04-28|07:45:36.752] Initialising Ethereum protocol           versions="[63 62]" network=1
INFO [04-28|07:45:36.768] Loaded most recent local header          number=0 hash=d4e567…cb8fa3 td=17179869184 age=50y1w6d
INFO [04-28|07:45:36.768] Loaded most recent local full block      number=0 hash=d4e567…cb8fa3 td=17179869184 age=50y1w6d
INFO [04-28|07:45:36.768] Loaded most recent local fast block      number=0 hash=d4e567…cb8fa3 td=17179869184 age=50y1w6d
INFO [04-28|07:45:36.768] Regenerated local transaction journal    transactions=0 accounts=0
INFO [04-28|07:45:36.799] New local node record                    seq=1 id=ee394bfe9ea8d8c5 ip=127.0.0.1 udp=30303 tcp=30303
INFO [04-28|07:45:36.802] Started P2P networking                   self=enode://558449d0f3c095adfcdbb32bd206c25acea89338fe1240f1aae8a8060e4f9d76732fc0654c811bba9dc9fcb88935166316cfe32fe5c5b2f9d141e81c4ee8cb1c@127.0.0.1:30303
INFO [04-28|07:45:36.803] IPC endpoint opened                      url=/Users/evan/Library/Ethereum/geth.ipc
INFO [04-28|07:45:36.805] HTTP endpoint opened                     url=http://127.0.0.1:8545                 cors= vhosts=localhost
```

這邊可以查詢到，本地端的 `geth` client 是跑在 8545 。只要修改 `truffle.js` 內的設定值，如此一來就可以執行了。

### 修復剩餘的測試錯誤



```
 9 passing (3s)
  1 failing

  1) Contract: Ballot Contract
       Valid individual votes:
     TypeError: Cannot read property 'call' of undefined
      at Context.<anonymous> (test/test.js:69:36)
      at web3.eth.getBlockNumber.then.result (/usr/local/lib/node_modules/truffle/build/webpack:/packages/truffle-core/lib/testing/testrunner.js:152:1)
      at <anonymous>
      at process._tickCallback (internal/process/next_tick.js:189:7)
```



原來是因為 getter 寫法有誤，需要參考以下方式修改。 (加上 memory 關鍵字) <https://ethereum.stackexchange.com/questions/64066/solidity-getter-and-setter-error>

```
function getCount() public view returns (uint[4] memory) {
        return proposals;
    }
```





# Reference

- [[TIL] Migrate solidity from remix to truffle (1) - Compile](http://www.evanlin.com/til-remix-truffle/)
- 