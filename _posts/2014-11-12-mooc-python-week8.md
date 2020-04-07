---
layout: post
layout: post
title: "[Mooc][Python][Week8]終於要完成了..... 完成最後的太空船射擊遊戲"
description: ""
category: 
- python
- 網路課程筆記
tags: []

---

![image](https://www.python.org/static/img/python-logo.png)

**前言與心得：**

前一次的作業雖然說是要做一個太空船射擊隕石的遊戲，但是僅僅是完成前半段．也就是只有完成船的移動，發射飛彈與隕石的移動部分．接下來就要做碰撞跟多個隕石的控制．

總算把這個課程弄完了，最後的作業其實讓我寫起來很過癮．這大概也是為什麼我除了平常晚上努力寫之外．週末還要拿時間來寫． 主要是最由透過去寫一個遊戲，來完成對於物件導向與多個物件的管理．

比如說，你要如何管理一個子彈飛行的時間跟是否有撞擊到目標．其實只要管理好一個，到多個的時候自然水到渠成．一步步慢慢地完成每個單獨的類別，然後看到多個物件瞬間可以完成的成就感真不錯．


**筆記:**

這一個章節主要是講解 List，Set與 Dictionary．而重心主要在Set上面．

- 關於List
    - 資料可以重複
- 關於Set
    - 不會重複
    - 在逐一讀取(iteration)的時候，不應該做新增跟移除的動作，會造成索引混亂（也就是會讀錯個或是少讀）    
        - 一般的作法就是另外列出一個集合後，利用差集合(difference_update)去運算．
        - 其中要注意difference不會改變原來的集合元素，而difference_update會．
    - 其中對於原來集合會有變更mutate的函式，有以下:
        - add/remove/modify/update(但是s.update(s)不會更改)
        - 兩個集合操作要有_update的函式

<pre class="prettyprint">
def get_rid_of(inst_set, starting_letter):
    #透過另外一個集合來搜集所有要刪除的
    remove_set = set([])
    for inst in inst_set:
        if inst[0] == starting_letter:
            remove_set.add(inst)
    ＃透過difference_update來移出所有在remove_set裡面的元素            
    inst_set.difference_update(remove_set)
</pre>        



**作業：**

其實上次不小心好像把這個禮拜的作業都弄完了．不過還是有一些細節需要微調:

- 由於這是一個太空船射擊遊戲，主要就是一個飛船在螢幕上可以發射飛彈來射擊每秒隨機產生的隕石．
    - 所以原先的做法是寫了許多的函式來處理撞擊與子彈飛行的部分，這先部分要拆解到原先飛船與隕石各自的類別來處理．
    - 對於個別物件的參數取得，原本做法比較粗糙就是直接取得．這部分得都改由函式來取得．
- 整個架構上，出題的人希望你透過兩個類別就能完成整個遊戲．一個類別是飛船，另外一個則是sprite．而不論是隕石，子彈或是爆炸三者都被視為是sprite的一個部分．
    - 其中就必須要區分出來這三者哪些有生命週期，比如子彈跟爆炸必須要多久之後消失，而隕石不需要．
    - 哪些屬於動畫需要改變畫圖的image


         
