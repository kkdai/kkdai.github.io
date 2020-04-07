---
layout: post
title: "[Coursera] Smart Contract (二）"
description: ""
category: 
- coursera
tags: ["coursera", "moocs", "blockchain", "ethereum"]

---

![](../images/2018/blockchain.png)



# [Blockchain Specialization](https://www.coursera.org/specializations/blockchain) 系列上課心得

- [Blockchain Basics](http://www.evanlin.com/moocs-bitcoin-basis/)
- [Smart Contract ](http://www.evanlin.com/moocs-smart-contract/)(本篇）
- Decentralized Applications (Dapps)


## Smart Contract 課程鏈結:  [這裡](https://www.coursera.org/learn/smarter-contracts/home/welcome)



# 文章鏈結:

- [Smart Contract (一）: Week 1 ~ week2](http://www.evanlin.com/moocs-smart-contract/)
- [Smart Contract (二）:Week3  ~ week4](http://www.evanlin.com/moocs-smart-contract2/) 
- [Smart Contract (三) : 期末作業](http://www.evanlin.com/moocs-smart-contract3/)

# 前言：

Smart contract 的課程進入了第二週，不是很確定能不能夠在兩週連最後的程式作業都能夠完成。（看起來很難 XD) 不過還是希望能夠多寫一些部分，由於最終程式作業的部分題目就高達四頁滿滿的說明，看起來可能會需要多一個禮拜。  XDD


# 課程內容:

這部分的課程其實還蠻有趣的，包含以下的部分:

- 了解 Smart Contract 的內容元素
- Smart Contract 的程式語言 Solidity 的語法與語義
- 如何透過 Smart Contract 來解決問題
- 透過 Remix ( A Web IDE of Solidity) 來建立與部屬你的 Smart Contract



## Week3:

### 回歸選票的智能合約

這個章節一開始就解釋關於 week2 的範例 Ballot 的內容。

- 這是一個類似選舉的 Smart Contract 
- 只有主席能夠註冊候選人，其他人只能投票。
- 主席一票代表兩分，其他人都只有一分。
- winningProposal 會取得誰當選的結果

<script src="https://gist.github.com/kkdai/b24552a125555073ef60d49acb2379da.js"></script>

### 開始改進

但是其實這個範例有著許多設計不夠周全的地方:

- 當沒有人註冊的時候， winingProposal 依舊會開票。
- 投票沒有時間限制
- 無法確定所有的選民都投票

#### 解決第一個部分的問題，沒有投票的狀態

透過 modifier 來解決

```

   modifier validStage(Stage reqStage)
    { require(stage == reqStage);
      _;
    }
```

以上是 modifier `validState` 的定義，可以透過以下的方式來在 function entry 來檢查。

```
function vote(uint8 toProposal) public validStage(Stage.Vote)  {
 ......
 }
```

可以看的到，透過 `validState` 可以檢查輸入的狀態是否為事先定義的 `regState` 。



#### 投票沒有時間限制

可以透過時間限制的方式來做，但是要如何在 solidity 裡面來檢查時間是否在有效期間內（事先定義為 30 seconds)

```
if (now > (startTime+ 30 seconds)) {stage = Stage.Done; votingCompleted();}
```

這邊也要註解一下:  `now` 是 solidity 的保留自，裡面代表的是當前這個 block 被建立的時間。

這邊可以查詢相關 `now` 的文件，可以更清楚。 https://solidity.readthedocs.io/en/develop/units-and-global-variables.html?highlight=block.timestamp#block-and-transaction-properties



#### 無法確定所有的選民都投票

比需要對每個 sender 來確認是否總票數有超過總人數，並且確定該 sender 並沒有過票。

```
 if (sender.voted || toProposal >= proposals.length) return;
 sender.voted = true;
 sender.vote = toProposal;   
 proposals[toProposal].voteCount += sender.weight;
```



### 關於各種不同的檢查方式在 smart contract 是否正常執行完畢

- **使用 if … return** 
  - transaction 會 mined ，而且會執行成功。
- **使用 modifier**
  - Translation 會 mined ，但是執行會失敗

檢查方式透過執行完畢後，查看 detail 結果就可以看到



### 關於 Events 的使用方式

根據這篇文章的說明 https://media.consensys.net/technical-introduction-to-events-and-logs-in-ethereum-a074d65dd6 ， events 可以分成以下三種類型：



#### 作為回傳資料的方式

<script src="https://gist.github.com/kkdai/59a0b05fb6ff9499b0410d4684101d50.js"></script>

根據這段範例，可以看得出來 contract `ExampleContract` 的 function `foo`  是透過:

```
event ReturnValue(address indexed _from, int256 _value);
```

是作為回傳資料之用。



#### 作為資料儲存之用

## Week4: Best Practices

接下來這個章節來探討，究竟什麼才是 smart contract 的最佳應用。



### Smart Contract 適合來解決 

- 分散式的問題
- 點對點交易
-  Operate beyond the boundaries of trust among unknown peers.
- Require validation, verification, and  recording on a universally timestamped, immutable ledger



### 一些 coding 小建議

- 不存放不相關的變數
- 透過 modifier 來確保是否具有能夠執行 function 的條件 (切勿使用 if … return)
- 只存必須的資料在 smart contract 之中
- 不要將大量資料放在 smart contract 之中
- 所有變數預設都是 private ，除非你特定加上 public 才能公開。
- Public 變數在編譯的時候會自動產生 getter method
- 將 events 使用在通知上面



### 在下列的時候使用 function access modifier :

- 負責規則，法規與守則
- 建立讀取 function 的基本守則
- 將 application 相關的規則套用上去
- 建立一個可驗證的元素可以用來檢視 smart contract



# 小結:

第三個禮拜跟第四個禮拜都開始深入使用 Solidity 一些比較重要的功能。包括了資料確認的 `modifier` 跟作為相關資料儲存或是回傳用的 `events` 。 這些東西才能夠顯示出來 Solidity 與 smart contract 跟其他語言與服務不同的地方。 

# Reference:

- [Technical Introduction to Events and Logs in Ethereum](https://media.consensys.net/technical-introduction-to-events-and-logs-in-ethereum-a074d65dd61e)
- https://solidity.readthedocs.io/en/v0.5.2/solidity-by-example.html?highlight=require
