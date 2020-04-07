---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-02-08 17:37:43+00:00
slug: c%e6%ba%ab%e6%95%85%e7%9f%a5%e6%96%b0-%e9%97%9c%e6%96%bc%e5%a4%9a%e9%87%8d%e7%b9%bc%e6%89%bf-inheritance-%e7%b6%93%e5%b8%b8%e7%99%bc%e7%94%9f%e7%9a%84%e5%95%8f%e9%a1%8c
title: '[C++溫故知新] 關於多重繼承 Multiple Inheritance 經常發生的問題'
wordpress_id: 1315
categories:
- C++溫故知新
- VC++程式設計
---




**Virtual Base Class**




* * *




多重繼承如下圖的時候




![](https://publib.boulder.ibm.com/infocenter/lnxpcomp/v8v101/topic/com.ibm.xlcpp8l.doc/language/ref/xcpvirt1.gif)




 




如果要使用  




     D *dptr = new D();




     D->L::para1 




會有compiler error :: Ambiguous conversion




因為實際的的compiler 會把 B1與B2 個編譯出一個L 為實體(class table)




所以要取用D 中的 L會有兩個目標出現




==> 解法： 使用virtual base class 在 B1 與 B2



    
    class L { /* ... */ }; // indirect base class
    class B1 : virtual public L { /* ... */ };
    class B2 : virtual public L { /* ... */ };
    




class D : public B1, public B2 { /* ... */ }; // valid




 




 




**Ambiguous Base Class:**




* * *

 一樣的Case但是在 B1 與 B2裡面都有命名 變數 nB




若是要使用




     D *dptr = new D();




     D->nB 




則會出現 compiler error




==>解法: 必須將要使用的 nB 定義清楚， 比如說 D->B1::nB 或是 D->B2::nB




 




**詳細程式如下:**





**參考資料：**









  * [https://publib.boulder.ibm.com/infocenter/lnxpcomp/v8v101/index.jsp?topic=%2Fcom.ibm.xlcpp8l.doc%2Flanguage%2Fref%2Fcplr135.htm](https://publib.boulder.ibm.com/infocenter/lnxpcomp/v8v101/index.jsp?topic=%2Fcom.ibm.xlcpp8l.doc%2Flanguage%2Fref%2Fcplr135.htm)


  * [https://publib.boulder.ibm.com/infocenter/lnxpcomp/v8v101/index.jsp?topic=%2Fcom.ibm.xlcpp8l.doc%2Flanguage%2Fref%2Fcplr135.htm](https://publib.boulder.ibm.com/infocenter/lnxpcomp/v8v101/index.jsp?topic=%2Fcom.ibm.xlcpp8l.doc%2Flanguage%2Fref%2Fcplr135.htm)







      
