---
layout: post
title: "[Coursera] Deep Learning Specialization:  Neural Networks and Deep Learning (一）"
description: ""
category: 
- coursera
tags: ["coursera", "moocs", "Deep Learning", "Neural Networks", "Machine Learning"]

---




![](https://tctechcrunch2011.files.wordpress.com/2017/06/ddbrg0vuwaelmop-1-jpg-large.jpeg?w=738)

## 起源

本來就想把 Deep Learning 學一下， 因緣際會下看到這一篇 [Coursera 學習心得](https://medium.com/@ywchen88/andrew-ng-deep-learning-specialization-%E8%AA%B2%E7%A8%8B%E6%84%9F%E6%83%B3-1-3-6acf4d6c5c82) 試讀了七天，除了提供 Jupyter Notebook 之外，作業也都相當有趣，就開始繼續學了． 目前進度到 Week2 相當推薦有程式設計一點點基礎就可以來學．裡面的數學應該還好． 學習的過程中還可以學會 Python 裡面的 numpy 如何使用，因為裡面主要就是要教導你如何使用 numpy 來兜出 Neural Network ．

### 課程鏈結:  [這裡](https://www.coursera.org/learn/neural-networks-deep-learning/home/welcome)

#### 學習鏈結:

- [Week 1-2: Introduction to deep learning & Neural Networks Basics](http://www.evanlin.com/moocs-DLS-NN-DL/)
- [Week 3: Shallow neural networks](http://www.evanlin.com/moocs-DLS-NN-DL2/)
- [Week 4: Deep Neural Networks](http://www.evanlin.com/moocs-DLS-NN-DL3/)



## 課程內容:

### 第一週: Introduction to deep learning

- 基本上就介紹 Machine Learning 與 Deep Learning 的基本常識． 比如說 (ReLU activation functionReLU activation function
- 介紹了近幾年 Deep Learning 會如此發展快速的原因 (計算速度的進展..)
- 最後有採訪 Geoffrey Everest Hinton (也就是將 Backpropagation 導入了 Deep learning 的人，並且發明了 Boltzmann machine ) ． 
	- 有不少有趣的事情，比如作者認為自己作出的  Boltzmann machine 很美麗，但是無法實作．
	- 所以後來作者加上了一些限制後，衍生出 restricted Boltzmann machine ，該系統就被 Netflix 拿來使用．

### 第二週: Neural Networks Basics

- 從頭開始教，一開始就教導你什麼是 binary-classification ．
	- 透過簡單的影像判斷是不是貓，來教導．
	- 透過不同的資料（R, G, B) 來判斷是不是貓．
- 教導一些 Deep Learning 的基本:
	- Logistic Regression 
	- Ligistic Regression Cost Function
	- Gradient Descent
	- Dervatives
- 透過一個 Jupyter Notebook 來教導你如何透過 numpy 來寫一個簡單的二元分類器 (classifier) 來分辨輸入的圖片是不是貓． 實際流程是:
	- 先做一個 sigmoid function
	- 再來實作 propagate function.
		- 拿到 X
		- 做出 forward propagate:
			- 算出 A = sigmoid($$w^TX+b)
			- 再來算出 cost function J = $$ -1/m \sum_{i=1}^m y^i * log(a^i) + (1-y^i)log(1-a^i) $$
		- 再來做出 backward propagate:
			- 計算出 dw 與 db.
			- $$ dw = 1/m (X (A-Y)^T) $$
			- $$ db = 1/m \sum_{i=1}^m (A-y^i) $$
	- 再來要做 optimize function
		- 透過 gradient descent algorithm 來優化 w, b
		- 透過 iteration 的 loop
			- 透過 propagate 來找出新版的 dw, db
			- 透過 dw, db 來修正目前的 w, b
				- $$w = w - learning_rate * dw $$
				- $$b = b - learning_rate * db $$
			- 進行下一回合的 propagate
	- 再來就要透過計算出來的 w, b 來預測 (predict) 是否是貓
		- 透過 $$ A =  sigmoid( (X * w) + b) $$
		- 如果 A > 0.5 (threshold) 我們就認為這張圖片是貓
	- 整個 Model 的部分，就是做以下的事情:
		- 初始化 w, b (在此是設定為 0, 其實有更好的方式)
		- optimize 找出 w, b
		- 透過 w, b 來跑 predict
		- 作 training accuracy 鑑驗

我必須得說，最後這個 Jupyter Notebook 真的太有趣了...	

#### NN 的常用符號解釋:

- $$m$$: 表示所有的訓練資料個數 (traning set)
- $$n$$: 每一個資料集中所具有的資料量
- $$ X \in \R $$ 並且 $$ X=(n_x, m)$$
- $$ Y $$ 代表是結果  $$ Y \in {0, 1} $$ 
	- (0: 代表不是，　1: 代表是)



#### 關於一些 numpy 的基礎教學

由於這次課程不會用到比較複雜的 NN (Neural Network) 套件，而是直接使用 numpy 來打造一個 NN ，這裡有不少的 numpy 教學部份．有提到一些類似於:

`import numpy as np`

- numpy 的 `a=np.random.randn(5)` 產生出來不是一個具有 transporm matrix 的 array 而是一個 `a.shape = (5,)`
	- 要正確產生一個 5,1 的 array 需要輸入 `a=np.random.randn(5,1)`
	- 建議加上 `assert(a.shape==(5,1))` 作為檢查，確認沒有漏打．

#### 透過 numpy 與 math 來學 sigmoid 

$$sigmoid(x) = \frac{1}{1+e^{-x}}$$

```
import math

def basic_sigmoid(x):
    sig = 1 / (1 + math.e ** -x)
    return sig
```	

由於 `math` 不能直接處理 array ，要改成 `numpy`

```
import numpy as np

def np_sigmoid(x):
    sig = 1 / (1 + np.exp(-x))
    return sig
```


#### usig numpy for norm

```
x_norm = np.linalg.norm(x, axis=1, keepdims=True)
normalize_x = x/x_norm
```

#### Softmax implement by numpy



```
def softmax(x):
    exp_x = np.exp(x)
    sum = np.sum(exp_x, axis=1, keepdims=True)
    s = exp_x/sum
    return s  
    
```    

#### numpy 的效能評比

針對 vector 比較大量的時候，其實 numpy 的效能反而比起直接透過 for-loop 來運算快得多．並且整個代碼都清楚多了．

#### L1, L2 loss implement by numpy

```
def l1(yhat, y):
    loss= np.sum(np.abs(y-yhat))
    return loss
    
def l2(yhat, y):
    loss2= np.sum(np.dot(y-yhat, y-yhat))
    return loss2
```

記住 `np.dot` 是將兩個矩陣相乘，所有要求平方就是自己跟自己相乘．        

#### 使用 numpy 來做 propagate

```
# GRADED FUNCTION: propagate

def propagate(w, b, X, Y):
    m = X.shape[1]
    A = sigmoid( np.dot(np.transpose(w),X) + b )                                   # compute activation
    cost = -1 / m * np.sum( np.multiply(Y, np.log(A)) + np.multiply(1-Y, np.log(1-A)))
    
    dw = 1/m * np.dot(X, np.transpose(A-Y))
    db = 1/m * np.sum(A-Y)

    assert(dw.shape == w.shape)
    assert(db.dtype == float)
    cost = np.squeeze(cost)
    assert(cost.shape == ())
    
    grads = {"dw": dw,
             "db": db}
    
    return grads, cost
```    

#### Numpy 裡面處理 matrix 需要注意的部分

- 加法可以直接處理 [3,2] + [3,1]
- 關於乘法(*) (跟 np.multiply 相同) 第一個維度需要相同 
	- [4,3]*[3,1] --> Error
	- [4,3]*[4,1] --> [4,3]
		- 註解： 此狀況會產生 broadcasting 
- np.dot(): 注意第一個的第二維度需要跟第二個的第一維度相同（矩陣相乘）
	- np.dot([4,3], [3,1]) --> [4,1]
	- np.dot([4,3], [4,1]) --> Error

#### Interview with Pieter Abbeel 

Pieter Abbeel 是處理 Deep Reinforcement Learning 的專家，這篇有提到他如何進行相關研究．