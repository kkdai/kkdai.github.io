---
layout: post
title: "[Coursera] Blockchain Basics"
description: ""
category: 
- coursera
tags: ["coursera", "moocs", "blockchain", "ethereum"]

---

![](../images/2018/blockchain.png)

## 課程鏈結:  [這裡](https://www.coursera.org/learn/blockchain-basics/home/welcome)



# [Blockchain Specialization](https://www.coursera.org/specializations/blockchain) 系列上課心得

- [Blockchain Basics](http://www.evanlin.com/moocs-bitcoin-basis/) (本篇）
- [Smart Contract ](http://www.evanlin.com/moocs-smart-contract/)
- 待續


# 前言：

最近開始決定來清理一下 Coursera 上面積欠的課程，但是也想到竟然有這幾天的假期，不如來好好的學習一些有趣的知識． Ethereum 就是一個我之前一直想好好學習，但是沒有時間可以了解的． 剛好看到 Coursera 也有相關的課程，所以決定把這個系列課程剛好這幾天學習了解．

「Blockchain Basics」 算是比較粗淺的入門課程，並沒有任何的 Programming 的部分，主要是做概念的講解與名次的解釋．



# 課程內容:

## Week1:

基本的 Blockchain 結構:

### UTXO

**UTXO(Unspent Transaction Output)**: 結構包含如下

- Unique identifier of the transaction that created the UTXO
- Index 
- Value
- (option): Condition under output can be spent

一個 Transaction 會有 Input UTXO 也會有 Output UTXO，



### Genesis Block

Genesis Block 指的是**第一筆的交易**，不論是 Bitcoin 或是 Ethereum 都有他們的 Block #0 ，我們可以透過以下的工具來查看：

- **Block Explorer**: https://blockexplorer.com/block/000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f
  - 具有 1 個 transaction
  - Reward: 50
  - Difficulty: 1
  - 時間： 2009 (如果你那時候開始玩..  )
- **EtherScan**: https://etherscan.io/block/0
  - 具有 8893 個 transactions
  - 0 個 contract
  - Gas: 5
  - Reward: 5



## Week2: 

### Smart Contracts:

**Smart contracts**  是透過類似 [Solidity](https://solidity.readthedocs.io/en/v0.4.24/) 的高階程式語言，寫成一段程式碼可以透過 EVM (Ethereum Virtual Machine) 跑在任何的 ethereum client 上面．

### Ethereum 貨幣單位 

$$
1 Ether = 10^{18} Wei
$$

### 交易產生 Smart Contracts 流程:

每一筆交易可以帶有可執行的程式碼 (smart contracts) ，但是執行該 Smart Contracts 的算力（Gas) 會被反映到手續費裡面．並且有嚴格的 Gas Limit 來掌控一個 Block 所能使用的上限．

### 節點功用:

- **Full node:**
  - 交易的啟動，認證
  - 挖礦
  - 區塊的建立
  - 智能合約的執行
- **Minor node:**
  - 接收，驗證並且執行交易



### Gas Point:

Gas Point 是給所有節點為了執行 Smart Contract 的手續費，計算範例如下:

- Step: 1
- Load from memory: 20
- Store into memory: 100
- Transaction base fee: 21000
- Contract creation: 53000

如果 Gas Point 不夠或是消耗完了，該筆交易就會被取消．



### Mining incentive model

Prove of work puzzle winner 拿到所有的 Block reward ，但是緊接在後面的 miner 叫做 Ommer miner ．而它們挖出來的 Block 也會放著作為 Security check 之死，並且拿到一小筆的 reward．



### Ethereum Accounts:

- **EOA(Externally owned account)**: 透過一組 PK (private key) 來代表你是帳號擁有者，並且可以執行相關操作．
- **Contract**:  透過相關 smart contract 的 code 來控制的帳號．

### Block creation sequence 

- 交易開始
- 交易認證 (Validation)
- 交易綁定與廣播
- PoW 解決 Puzzle
- 增加新的 Block 到鍊上

Refer: [Life cycle of ethereum transaction](https://medium.com/blockchannel/life-cycle-of-an-ethereum-transaction-e5c66bae0f6e)



## Week 3:

### ECC (Elliptic curve cryptographyphy)

Strong than RSA, 

```
256 bits ECC key pair ~= 3072 bits RSA key pair 
```

### Hash function

- **Simple Hashing:**
  - Fixed number of items (block header)
- **Tree hashing:**
  - Number of items differ from block to block

#### All hashing output:

- Account address
- Digital signiatures
- State hash
- Receipt hash

#### 如何保證 block 的一致性 (Integrity)

- 不可串改的 block header
- Transaction 不可被串改
- State 的移轉是透過計算Hash 編碼並且認證過的

**Bitcoin  與 Ethereum 都是透過 SHA-3 演算法 Keccak 來作加密**

#### Bitcoin 如何計算這個 block 的 hash value? 需要以下資訊:

- Previous block hash
- Version
- Merkle Root Hash
- Timestamp
- Bits
- Nonce

參考網址：  https://cse.buffalo.edu/blockchain/blockhash.html

#### Ethereum Merkle Tree 

用來作用比對交易的一致性 (integrity)， leaf node  存放交易(transaction)的資料． Parent node 則存放該子結點的 Hash Value. 而 Root node 存放所有交易的 Hash Value. 這樣做的好處有:

- **一致性檢查**: 所有的 Hash value 環環相扣，不容易被人輕易修改．
- **修改少**: 如果一筆交易有修改，要修改得資料只有由這個子結點一路到 root node

參考: 

- https://www.youtube.com/watch?v=h1wzzhkHfTk
- https://bitcoin.stackexchange.com/questions/10479/what-is-the-merkle-root

## Week4:

### Trust Trail flow:

- 驗證Transaction
- 驗證 gas 跟 資源(resources)
- 選擇一群交易 (transactions) 來建立新的 block 
- 執行該 Transaction 來取得新的狀態 (state)
- 建置一個新 block
- 通過 consensus 驗證
- 將 block 加入到 chain 的最尾端

### Consensus of blockchain

- https://www.coindesk.com/short-guide-blockchain-consensus-protocols/
- https://blog.wavesplatform.com/review-of-blockchain-consensus-mechanisms-f575afae38f2

### Double Spending

- 指的是有兩個 block 在非常接近的時間產生出來．這樣的話兩個 block 會同時連接到 main chain 產生兩條 chain ． 但是之後比較長的 子鍊 會取代成為主鍊

- Ethereum 透過 Global nonce 跟 account number 來解決這樣的問題．

### PoW v.s. PoS

這段影片很簡單扼要

<iframe width="560" height="315" src="https://www.youtube.com/embed/M3EFi_POhps" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

簡單記錄一下:

- **Proof -of-Work**:  (Bitcoin, Ethereum ( 主鍊))
  - Miner 透過消耗算力來解開 puzzle ，驗證是否為適合的 block. 
  - Miner 得到相對應的 reward
  - **風險:**
    - 51% 攻擊，當有人能夠集結 51% 以上的算力的時候就可以偽造資料，修改 main chain

- **Proof-of-Stake**: (目前用在 Ethereum testnet)
  - 所有總資產一開始就存在，而每筆交易需要手續費（賭注）
  - 沒有人負責 mine block 而是作為 validate block
  - 每個人都可以附上一筆手續費來提升自己被選為下一個 block 的費用（賭注）
  - **風險:**
    - 只要有人能夠拿出 51% 的錢當手續費，就有機會攻擊．這部分有更多論文正在討論．

### Forks:

- **Soft fork**:  Software patch
- **Hard fork**: Protocol change which introduce two chain could not merged.

#### Soft-fork in Bitcoin

Bitcoin 有過一次 Soft-fork for P2SH Script ( pay-to-script-hash script) refer here https://bitcoin.org/en/developer-guide#p2sh-scripts

#### Hard-fork in Ethereum (Ethereum Homestead --> Ethereum Metropolis)

包括了以下的 EIP (Ethereum Improvement Proposal):

- Parallel processing of transactions
- Proof-Of-Works consesnsus still stay except that every hundren blocks, Proof-of-stake consensus protocol is applied for evaluating latter.
- Miner incentive was reduce from 5 Ethers to 3 Ethers. 



## Final Course Project

最後的作業，會讓你跑一個本地端的 VM 內建已經安裝好的 Ethereum client (geth) ．並且跑一系列動作新建節點，連線，挖礦，交易．簡單但是可以馬上感受到 Ethereum 的完整感受．

# Reference:

- http://www.takethiscourse.net/blockchain-online-courses-moocs/
- https://medium.com/blockchannel/life-cycle-of-an-ethereum-transaction-e5c66bae0f6e
- https://github.com/ethereum/wiki/wiki/Ethereum-Development-Tutorial
- https://cse.buffalo.edu/blockchain/blockhash.html
- https://medium.com/@fukuball 




