---
layout: post
layout: post
title: "[MOPCON2014]蒐集一些有趣的MOPCON場次(1)"
description: ""
category: 
- 研討會心得
- python

tags: []

---

![image](https://lh3.ggpht.com/WENOY8ipns5yTuCn1xOW86e9vZVIMOvUXAbIxMvrUVzgWisjOR5mGOfn1yYmzz9V6zQ=w300)


**前言：**

雖然我沒有去[MOPCON2014](http://mopcon.org/2014/session.php)，但是還是有去看一些有趣的題目，順便找找他的slide出來分享一下． 這裏有全部[議題](http://mopcon.org/2014/session.php)．


**有趣的題目:**

- **講個秘訣之：離開新手村後也可以順便聽一下的程式「設計」指南**
    - **Slide**: 
        - [http://www.slideshare.net/excusemejoe/programming-x-design-mopcon-2014](http://www.slideshare.net/excusemejoe/programming-x-design-mopcon-2014) 
    - **內容與心得**：
        - 要提到程式設計中"設計"的思維．既然我們都稱自己是程式設計師，那麼你的作品是不是真的符合所謂"設計"的概念？
        - 裡面提到的設計心理學，如果將它應用到程式設計:
            - 預設用途: 
                - 設計任何程式之前，對於他的預設用途必須要先想清楚．
            - 指意: 
                - 要能讓別人一看就能了解並且不容易搞混的API設計．                
            - 使用侷限:
                - 設計各種功能與API要注意到可能會有許多使用上的侷限．不論是[物理][文化][意義]或是[邏輯]上．
            - 對應性:
                - 設計的時候要有好理解的對應性，才能讓人馬上上手．           
            - 回饋:
                - 設計適當的回饋，可以讓使用的人了解到目前的狀況．
            - 概念模型:
                - 高度簡化下，對於事物的如何運作的解釋．
        - 如同講者提到的，其實任何一種設計都是圍繞著人走．所以就算是程式設計也要圍繞者使用的人為出發點去思考，能能算是比較好的設計觀念．
                    
- **in in der 響應式編程**
    - **Slide:** 
        - [http://www.slideshare.net/jingtw/in-in-der](http://www.slideshare.net/jingtw/in-in-der)
    - **內容與心得**
        - FRP (Functional Reactive Programming)
            - FP + RP
            - 賦予 RP 有 FP 的函式                

<pre class="prettyprint">
// Impreative Programming
int x,y,z;
x=1; y=2;
z= x+y; //1+1
y= 2;
print("%d", z); //z=2

// Reactive Programming
int x,y,z;
x=1; y=2;
z= x+y; //1+1
y= 2;
print("%d", z); //z=3
 
</pre>

                     
- **Best software architecture in app development**
    - **Slide:** 
        - [http://www.slideshare.net/anistarsung/mopcon-2014-best-software-architecture-in-app-development](http://www.slideshare.net/anistarsung/mopcon-2014-best-software-architecture-in-app-development)
    - **內容與心得:**
        - 如何容易地展現內容？ 建議使用 ListView其中 UICollectionView是功能強大的部分．
            - 優點：
                - UICollectionView 可以自訂顯示方式，並且可以對應到不同的大小顯示方式．
                - Cell 可以重複用，具有彈性的顯示方式
                - 具有自動更新與記憶體管理的部分
        - 修改一次可以同時變更許多地方: (主要提到將一些常用到的顯示部份抽取出來)
            - 顏色:
                - 將顏色變成類別內的變數[參考下面的程式碼(1)]
        - UI Layout 建議使用Xib 而不是Stroyboard
            - 原因： Storyboard 不易閱讀，而且開很大的時候相當耗費時間，並且時常有衝突不好修改．                
        - Unit Test: 建議使用XCTest Framework
            - 可以使用內建的Automation 來達成自動測試，就算是GPS相關的問題也可以透過GPX Creator 來測試．
        - 顯示你的內容
            - 利用CoreData ，最好用MagicalRecord
            - Lazy Loader 來顯示image 而不去阻擋UI thread [https://developer.apple.com/Library/ios/samplecode/LazyTableImages/Introduction/Intro.html](https://developer.apple.com/Library/ios/samplecode/LazyTableImages/Introduction/Intro.html)
            - Data Model 來表現MVC                     
<pre class="prettyprint">
//(1)
+ (UIColor *) themeBackGround
{
    return [UIColor colorWithHexString:@"dedede"];
} 
</pre>                                 



未完待續.....
