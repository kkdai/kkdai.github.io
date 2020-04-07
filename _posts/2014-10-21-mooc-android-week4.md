---
layout: post
layout: post
title: "[Moocs]Programming Mobile Aplication on Android Platform(week4) - All about UI"
description: ""

category: 
- 網路課程筆記
- android安卓學習心得
tags: []

---

![image](../images/2014/Android_ICON.jpg)

第四個禮拜主要就是介紹如何使用各種UI Control，比如說 Button，Ratio Control，以及這種Layout的使用方式． 而第四個禮拜的作業也相當有趣，就是寫一個基本的TODO item App．裡面包含了許多的關於 inflate與 findViewById的用法，也有講到如何控制各種簡單的TextView或是Radio 控制，更可以了解到利用Intent傳送資料的方法．

挺有趣的章節....


**筆記：**

**inflate() 與 findViewById()的差異**
- inflate是用來找尋layout的，可以利用他去找出整個Layout的內容，內容如下
    
<pre class="prettyprint">
    View view1=View.inflate(this,R.layout.dialog_layout,null);  
</pre>

- 之後就可以利用 view1 繼續去把每一個button 或是 control 透過 findViewById()找出來

<pre class="prettyprint">
    Button action_buttion = (Button)view1.findViewById(R.id.acton_button);
</pre>

**如何新增footerview到listview**
- 使用以下的方式 

<pre class="prettyprint">
    getListView().addFooterView(footerView);
</pre>

**參考資料:**

- 中文介紹 inflate 與 findViewById 差異(1)
    -  [http://blog.csdn.net/zmissm/article/details/13006521](http://blog.csdn.net/zmissm/article/details/13006521)
- 中文介紹 inflate 與 findViewById 差異(2)    
    - [http://www.cnblogs.com/loyea/archive/2013/04/27/3047248.html](http://www.cnblogs.com/loyea/archive/2013/04/27/3047248.html) 

