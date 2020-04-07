---
layout: post
title: "[TIL] 為何圍棋AlphaGo贏了人類有那麼重要?"
description: ""
category: 
- TodayILearn
tags: ["TIL", "machine_learning", "deep_learning"]

---

![](http://pics.ctitv.com/wpimg/2016/03/112.png)

## 前提:

要跟部門大長官開會前，他提到了目前受歡迎的AlphaGo事件．提醒我們對於新聞事件的思考邏輯應該是?

- 這個新科技是什麼？
- 為什麼之前做不到? 這次的突破點是什麼?
- 之後有可能的路是什麼?

這個思考脈絡很能夠引發對於科技更深一步的思考，於是找了一些資料試著把這件事情搞清楚．

裡面有幾個前提是:

- 我並不會下圍棋
- 主要參考文章是 ["尹相志Allan's blog- 淺談Alpha Go所涉及的深度學習技術"](https://dotblogs.com.tw/allanyiin/2016/03/12/222215)，還有幾篇相關的論文．

## 幾個可能的Q&A:


### 為何電腦下圍棋很難?

因為圍棋可能性是19X19格子，共有361個落子點．所以計算出來需要10的121次方.. (遠遠大過古代宇宙原子說  10的75次方) 

以西洋棋(比較簡單)來說40個分支，20步就算是1GHz的處理器，也要計算3486528500050735年． 所以就知道電腦要下圍棋在之前是不行的．

### 為何AlphaGo 這次能贏? 突破點是什麼?

#### Machine Learning 的突破點

Deep Learning突破點 2006 的是「A fast learning algorithm for deep belief nets」類神經網路的亂數權重不是隨機，而是經過計算．如此一來可以透過計算的權重刪減掉許多的子樹，大幅度地減少類神經網路的尋找解答的範圍．

之前下西洋棋的"深藍"取決法是透過[MinMax](https://en.wikipedia.org/wiki/Minimax)(也就是透過賽局理論)來選取最好的下法跟[Alpha-Beta Pruning](https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning) 來刪除不必要的搜尋路徑（不會勝利的選擇)．

#### AlphaGo 如何獲勝

而AlphaGo 思考模式.. 兩個類神經網路(人家稱為兩個大腦)(Policy Network, Value Network)

- Policy Network(策略類神經網路):
	- 猜測對方落子的機率．是一種大量輸入棋譜的監督式學習(透過猜測，並且給予解答認證)．
	- 由於透過整體形勢來做預測，亂下是無法打亂電腦的計算．
	- 透過增強策略網路(兩兩對戰，增加預測機率)，減少誤判．
- Value Network (評價網路):
	- 評估目前局勢，找出如何下會取得比較高的勝率．

最後的拼圖，[蒙地卡羅搜尋樹](https://en.wikipedia.org/wiki/Monte_Carlo_method):

- 選取:
	- 隨機挑選一個結果(由於兩個大腦挑選過，可挑選的分支變得可以計算）．
- 展開:
	- 透過剛剛開始的結果，推導接下來棋局．
- 評估:
	- 計算這次推導結果（樹的搜尋結果)
- 倒傳導:
	- 挑選最佳結果後，將結果回傳到一開始，然後繼續之後的計算． 


### AlphaGo贏了.. 接下來代表著什麼？

這部分留著繼續思考....

## 參考資料:

- ["尹相志Allan's blog- 淺談Alpha Go所涉及的深度學習技術"](https://dotblogs.com.tw/allanyiin/2016/03/12/222215)
- [2006: A fast learning algorithm for deep belief nets](https://www.cs.toronto.edu/~hinton/absps/fastnc.pdf)
- [Wiki: MinMax](https://en.wikipedia.org/wiki/Minimax)
- [Wiki: Alpha-Beta Pruning](https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning)
- [Wiki: Monte Carlo method](https://en.wikipedia.org/wiki/Monte_Carlo_method)
