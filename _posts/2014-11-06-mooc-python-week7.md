---
layout: post
layout: post
title: "[Mooc][Python][Week7] 太空船射擊遊戲"
description: ""
category: 
- python
- 網路課程筆記
tags: []

---

![image](https://www.python.org/static/img/python-logo.png)


**前言:**

自從課程開始忙碌之後，我已經把Android跟Python的順序開始做個調整．不過因為這堂課程的loading一直不算輕鬆，雖然他的主要課程內容圍繞著他所提供的simplegui與[codeskulptor](http://www.codeskulptor.org/)之外，他的作業其實都是屬於peer assignment．也就是說你除了寫好作業之外，你還需要改其他同學的作業，不然就像我這個禮拜一樣被扣了20%的分數 orz ．

**課程筆記:**

- 這次主要是圍繞著如何利用聲音播放與一些OnDraw的相關應用去做出一個太空船射擊的遊戲．這邊其中就會牽扯到一些部分需要瞭解：
    - 關於推動力與摩擦力的運算
    - 關於聲音的播放
    - 對於動態物件的表示，與封裝．比如說隕石跟太空船都是會動的物件，但是隕石會飛出畫面外，太空船則不會．
    - 對於畫面上物件的碰撞計算，前幾天在Android的作業裡面也是一樣的東西．     
- 利用Dictionary 達成variable point 或是 function pointer map．
    - 裡面的programming tip是每個禮拜課程中，都會教導一下關於這個禮拜可能會遇到的一些小技巧與需要注意的地方．
    - 這次提到透過dictionary 裡面去加入其他函示的方式來達成function map的作用．當然舉一反三也可以指向其他的類別



<pre class="prettyprint">
#關於其他function point mapping
def f():
    pass

d = { 0 : f }
d[0]()   # call f function.    



#利用Dictionary 做到類別mapping
class C1:
    def __init__(self):
        self.inVal =  5
        
    def get(self):        
        return self.inVal
    
obj1 = C1()
print str(obj1.get()) #5

d = { 0:C1 }

obj2 = d[0]()
print str(obj2.get()) #5

</pre>


**作業筆記:**

作業不簡單，所以寫了一下子才能回來更新部落格．一些筆記如下，大部分與[http://www.codeskulptor.org](http://www.codeskulptor.org)裡面的simplegui處理有關:

- 關於圖片顯示，原來的圖片都是PNG所以已經能透明，只要需要把圖片換成網路上png自行會處理透明的部分
- 先試著處理keyboard event 對應著圖片旋轉的部分，這時候會發現圖片預設是0度(degree)，所以要處理一下．
- 要寫這個作業，首先要先來複習一下角度(degree)與弧度(radians)．這邊我弄了很久因為我把角度(degree)的起始點弄錯，所自然算出來的弧度(radians)就是錯的．  

![image](http://math.rice.edu/~pcmi/sphere/degrad.gif)

<pre class="prettyprint">

#算出推進力，課堂的影片有提到
forward = (cos(radians) ,  sin(radians) )

</pre>


**參考資料:**

- 角度與弧度的運算
    - [http://math.rice.edu/~pcmi/sphere/drg_txt.html](http://math.rice.edu/~pcmi/sphere/drg_txt.html) 
    
