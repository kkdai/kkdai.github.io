---
layout: post
title: "[Coursera] Deep Learning Specialization:  Neural Networks and Deep Learning (二）"
description: ""
category: 
- coursera
tags: ["coursera", "moocs", "Deep Learning", "Neural Networks", "Machine Learning"]

---




![](https://tctechcrunch2011.files.wordpress.com/2017/06/ddbrg0vuwaelmop-1-jpg-large.jpeg?w=738)


第三週拖得有點久，因為被抓去專心寫 code 啦 (吃手手

第三週的課程其實很有趣，學到了 back-propagation 也學到了一些 NN 的技巧

- 如何有效設置初始權重
- Activation function 該如何挑選

當然 Jupyter Notebook 也相當的好玩啊...

最後的大師採訪，是訪問 GAN 的發明者 Ian Goodfellow 

## 起源

本來就想把 Deep Learning 學一下， 因緣際會下看到這一篇 [Coursera 學習心得](https://medium.com/@ywchen88/andrew-ng-deep-learning-specialization-%E8%AA%B2%E7%A8%8B%E6%84%9F%E6%83%B3-1-3-6acf4d6c5c82) 試讀了七天，除了提供 Jupyter Notebook 之外，作業也都相當有趣，就開始繼續學了． 目前進度到 Week2 相當推薦有程式設計一點點基礎就可以來學．裡面的數學應該還好． 學習的過程中還可以學會 Python 裡面的 numpy 如何使用，因為裡面主要就是要教導你如何使用 numpy 來兜出 Neural Network ．

### 課程鏈結:  [這裡](https://www.coursera.org/learn/neural-networks-deep-learning/home/welcome)

#### 學習鏈結:

- [Week 1-2: Introduction to deep learning & Neural Networks Basics](http://www.evanlin.com/moocs-DLS-NN-DL/)
- [Week 3: Shallow neural networks](http://www.evanlin.com/moocs-DLS-NN-DL2/)
- [Week 4: Deep Neural Networks](http://www.evanlin.com/moocs-DLS-NN-DL3/)

## 課程內容:

### 第三週: Shallow neural networks

關於多個 Neural Network 的表達方式:

第一層: $$z^[1] = w^[1]X+b^[1] $$

第一層輸出:  $$ a^[1]= sigmoid(z^[1]) $$

第二層: $$z^[2] = w^[2]a^[1]+b^[2] $$ 

第二層輸出:  $$ a^[2]= sigmoid(z^[2]) $$

下標代表的是第幾個 Neuron 

不同的輸入訓練資料 $ 1 ... m $$ 

```
for i = range(1, m):
	#第一層
	#第一層輸出
	#第二層
	#第二層輸出
```

#### 關於 Activation Function 的選用部分

這邊有篇文章很推薦"[26种神经网络激活函数可视化](https://www.jiqizhixin.com/articles/2017-10-10-3)"對於 Activation Function  有很多的著墨，在此筆記一下:

- Sigmoid 對於二元分類監督是學習（也就是學習是不是某種物品，比如說是不是貓) 相當的有用．因為出來的數值是 0~1 的數值，你可以根據學習狀況給予一定的 Threshold
- Tanh 會出現一個 -1~1 之間的數值，這樣學習可以變得更快．也可以但是對於二元分類的監督式學習不會比較好，於是也越來越多人將 Tanh 取代 Sigmoid

#### 關於權重 (weight) 的初始化

在 NN 中，當你具有一個以上的 hidden layer 時，就必須要慎重的初始化你的起始權重 (w1) ． 因為如果你的起始權重不是使用"**亂數**" 來作為初始化的話． 你的 hidden layer 的意義就不大，那是因為:

- W1 = [0, 0, ...] 那麼  `A1 = X1 * W1 +b1` , `A2=A1*W2 + b2` --> 當你的 b1, b2 也是 0 時．就會得到 A1=A2 

- 在做 back-propagation 也會沒有差別，造就整個 hidden layer 就沒有它存在的意義．

### 關於 Peer Assignment

這次的作業是做一個具有 back-propagation 並且具有一個 hidden layer 的 NN (當然依舊是使用 numpy )

但是跟之前不太一樣的部分是除了要完成 forward-propagation 之外，也必須要完成 back-propagation ． 並且在 forward-propagation 中除了之前用過的 activation function (sigmoid) 之外也有使用到 (tanh) 的 activation function

### 採訪的部分 - GAN 發明者 Ian Goodfellow

這一篇訪問其實相當有趣，訪問到最近相當熱門的 GAN (生成對抗網路) 的提出者 Ian Goodfellow 來討論．當然， Ian 也是 Andrew Ng 的學生之一（原來大家都跟 Andrew 有關係） 

裡面有提到， GAN 是他們在偶然討論中想出來的理論．但是 Ian 卻用一個晚上就把相關代碼準備好了．不過他自己也表示說，雖然 GAN 代碼可以很快完成，但是需要一個穩定的 GAN 卻是花費不少時間來讓他論文完整．

此外，大家應該也很好奇在那麼年輕就提出了 GAN ，並且寫了一本 [Deep Learning 的書籍](http://www.deeplearningbook.org/)（就叫 "Deep Learning")，他的下一步會是什麼？

Ian 提到，他對於 Security on NN 相當有興趣．也就是如何避免使用一些假資料（或是惡意的資料）來讓 NN 失去準確度甚至是被惡意的判別錯誤．

很有趣的訪問，很推薦一看．

	