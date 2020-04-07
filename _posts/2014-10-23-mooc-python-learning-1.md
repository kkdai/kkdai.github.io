---
layout: post
layout: post
title: "[Mooc][Python]開始上課Interactive Programming in Python(第一部分) week 0~week5"
description: ""
category: 
- python
- 網路課程筆記


tags: []

---


![image](https://www.python.org/static/img/python-logo.png)

**前言：**

其實這堂課之前有開始學，不過因為之前卡著學Scala而放棄．這次希望能一併學完它．先放上前五週的心得跟筆記．

**筆記：**

- 好用的codeskulptor [http://www.codeskulptor.org/](http://www.codeskulptor.org/)
    - 這個東西真的很方便，可以step debugging 之外，還可以視覺化目前(Viz Mode)的記憶體的狀況．
    - 使用方法:  點下Viz Mode -> 按下設定的按鈕 -> 按下單步執行的按鈕．
- 應該知道的基本常識:
    - 除法要保留小數點，記得要把被除數變成浮點數． (ex: 1/4 = 0 --> 1.0/4 = 0.25)
- Python也是具有類似指標意義的，可以兩個變數名稱指向同一份資料
<pre class="prettyprint">
    a = b = [1,2,3,4,5]
</pre>
- 關於Global 與 Local 的筆記:
    - Valraible 需要加上Global才能存取到Global
    - List 不需要加上Global 可以存取某個元素
    - 但是如果是給值整個list 不使用Global會存取到新的List  
- 關於List的操作  
    - 可以用 my_list + [1,2]  來做list 操作
    - append，extend 跟 reverse 都會改變source list
    - 取出可以使用pop (會變動)，或是使用 my_list[index]．其中index 若為負值是由最後開始算．
- 關於Dictionary 的操作:
    - Key 可以是 tuple，number，string．但是**不可使用list** (但是可以使用list的value)
    - Value 可以是 tuple，number，string．**甚至是list或是dictionary**
    - Key跟Value都不能包含，另外一個Dict
    - dict travesal 可以使用以下的方式
<pre class="prettyprint">
lista = [1, 2, 3, 4]
dict = { 1: 'x', 'x':2, lista[2]: lista, (2,3):(5,2), 77: {1:2, 2:3} }
'''
Using key to find dictionary value
'''
for key in dict:
    print dict[key]

'''
Using tuple ()key,value) to find dictionary items
'''
for key,value in dict.items():        
    print "(", key, ",", value,")"
</pre>    


**關於作業:**

作業都偏向跟使用者UI互動有關，其中跟Python原理應用比較少．大多圍繞在simplegui

- 第二週: 一個比猜拳更複雜的猜拳遊戲:
    - 總共有五個型態，考驗random 跟判斷
- 第三週: 猜數字
    - 就是0~100猜一個數字，然後告訴你太高或是太低
- 第四週: stopclock
    - 重點是在timer的應用
- 第五週: pong (乒乓球)
    - 一樣是考驗timer 與碰撞的畫圖部分應用
                    
