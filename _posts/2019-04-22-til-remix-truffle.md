---
layout: post
title: "[TIL] Migrate solidity from remix to truffle (1) -  Compile "
description: ""
category: 
- TodayILearn
- vscode
tags: ["TIL", "blockchain", "solidity", "truffle"]
---



![](../images/2019/truffle.png)



# 前提

最近在上Coursera 的 [Blockchain Specialization](https://www.coursera.org/specializations/blockchain) 系列的課程 [Decentralized Applications (Dapps)](https://www.coursera.org/learn/decentralized-apps-on-blockchain)，裡面提到使用 Truffle 。 由於課程裡面使用的 Truffle 版本比較舊（目前是 0.5.1，課程內是 0.4.0? ) ，本來想打算使用 VM 的方式來跑，但是發現現在的筆電跑 VM 有點悲情。 

只好硬著頭皮來直接去修改相關的代碼，想想相關的修改應該也可以變成一系列的文章。 就來試試看吧！！



# 內容分享

### Truffle 的介紹 

#### Truffle 與 Remix 的差異

這次主要會用到的工具叫做 [Truffle](https://truffleframework.com/) ，在使用 [Truffle](https://truffleframework.com/)  之前需要分別出跟之前使用 [remix](http://remix.ethereum.org)  的差別：

- 分析問題， Smart Contract 的 prototype 需要使用 [remix](http://remix.ethereum.org) 。
- 開發，測試與部屬 dApps 需要使用 [Truffle](https://truffleframework.com/)。

#### 如何透過 Truffle 部署簡單的專案

接下來透過  [Truffle](https://truffleframework.com/)要來跑一開始最簡單的範例 Ballot 流程如下：

- 安裝  [Truffle](https://truffleframework.com/) : 
  - `npm install -g truffle`
- 初始化專案
  - `mkdir ballot`
  - `cd ballot`
  - `truffle init`

裡面會有幾個已經設定好的 template 包括了:

- `migrations/1_initial_migration.js`: 負責 migration script file
- `truffle-config.js`: 負責 truffle compiler 的設定檔案

增加設定檔案 `truffle.js` (**這檔案是負責 truffle 部署的設定檔案**)

<script src="https://gist.github.com/kkdai/7052592fc31f355a63c2ce90233d7e98.js"></script>

增加 deployment 的相關設定 (建立一個檔案 `migrations/2_deploy_contracts.js`)

<script src="https://gist.github.com/kkdai/314224460e08304dc385fe997123a3d0.js"></script>

### 將之前的範例 Ballot.sol  migration 過來

這一段是 （[Smart Contract ](http://www.evanlin.com/moocs-smart-contract/) ) 課程中搬過來的第一個範例程式 ballot.sol ，大家也可以直接參考以下的部分。

<script src="https://gist.github.com/kkdai/f9d958ab8af73bb0581f4832ba8a8ae4.js"></script>  

檔案記得要建立在 `contracts/ballot.sol` 下面，接下來會講解如何做正確的 migration 。

#### 試著編譯 truffle

`truffle compile`  結果發現了一些錯誤，就來開始解決問題吧。

```
Compiling your contracts...
===========================
Error: CompileError: ParsedContract.sol:43:39: ParserError: The state mutability modifier "constant" was removed in version 0.5.0. Use "view" or "pure" instead.
    function winningProposal() public constant returns (uint8 _winningProposal) {
                                      ^------^
```



####先試著降 Solidity Compiler 版本

Truffle 0.5.1 使用到的 Solidity compiler 預設是 0.5.0 ，我們可以透過修改 `truffle-config.js` 來降低 solidity compiler 的版本。 可以參考: <https://truffleframework.com/docs/truffle/reference/configuration#compiler-configuration>



不過出現的問題卻更難解決:

```
Compiling your contracts...
===========================
Error: CompileError: ParsedContract.sol:7:14: Error: Expected identifier, got 'LParen'
  constructor() public {
             ^
```

看起來更看不懂，那麼換個方式。來幫現在的 Solidity code migrate 從 0.4.0 到 0.4.23 。



#### 開始進行 Solidity 升級的步驟 (0.4.0 —> 0.4.23):

- 先將版本訂在 0.4.23 以上：
  - `pragma solidity >=0.4.23 <0.6.0;`
- 因為之後 constrctor 語法不同，改一下： (可以[參考這個](https://solidity.readthedocs.io/en/v0.4.24/contracts.html))
  - `constructor(bytes32 _name) public {`
- `constant` 在 0.4.17 後不支援，改成 view
  - `function winningProposal() public view returns (uint8 _winningProposal) {`

細節大家可以參考這個 coomit. <https://github.com/kkdai/truffle_balot/commit/cc23ca5d97ecef48921c3b83c66a0763e7e43edc>



# 總結:

這裡先整理第一階段的 Truffle compiler migration ，接下來還有`truffle test` 與  `truffle migrate` 相關的整合部分會加入。這個相關過程也讓我更了解 truffle 運作的原理。

# Reference

- <https://truffleframework.com/docs/truffle/reference/configuration#compiler-configuration>
- <https://solidity.readthedocs.io/en/v0.4.24/contracts.html>