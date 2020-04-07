---
layout: post
layout: post
author: kkdai
comments: true
date: 2012-08-29 10:07:50+00:00
slug: ios%e9%97%9c%e6%96%bctableview%e5%9c%a8programing-%e7%9a%84%e4%b8%80%e4%ba%9b%e5%bf%83%e5%be%97
title: '[iOS]關於TableView在programing 的一些心得'
wordpress_id: 1168
categories:
- iOS的程式開發
- 學習文件
---

在開始寫iOS的程式開始，我一直對於整個Cocoa mvc的架構不習慣。




可能是之前設計Windows 的關係，許多的新的架構與方式會需要用很多的心力來了解。




在這裡試著記錄一些關於TableView的心得：




 




**利用New File建立第一個TableView所遇到的第一個新增資料的crash：**




一開始利用Storyboard來建立第一個TableView範例的時候，最容易遇到的就是crash在cellForRowAtIndexPath。




裡面最大的原因就是，如果你自己在Storybord上面建立一個TableViewController，並且透過新增ObjectC++ file來新增對應的ViewController.m




裡面就會發生一個問題就是Cell identifier的問題，解決方法有兩種：






  * 在storyboard上面把cell的identifier 加上去預設名稱"cell"


  * 或是在程式碼裡面有error handle如下 




 if (!cell)   
{          
   cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:@"My cell"];  
}




 




**關於TableView資料處理概念：**




iOS的TableView處理與撰寫Windows table的處理最大的不同就是對於資料處理的邏輯。




相較於Windows是一次把資料加入或是一筆筆新增或是刪除的方式。iOS必須得依照以下方式給資料






  * 必須先給你有幾個section (也可以當成是有幾個小表格）[numberOfSectionsInTableView]


  * 一個表格裡面有多少資料(row)  [numberOfRowInSection]


  * 確定之後，他會每一比資料都來問你裡面的內容 [cellForRowAtIndexPath]




  





**利用deleteRowAtIndexPaths刪除含有CoreData的TableView欄位，會出現Crash:**




這個問題也找了一下，詳細的程式碼與問題可以參考[http://stackoverflow.com/questions/4057199/animating-row-deletion-in-uitableview-with-coredata-gives-assertion-failure](http://stackoverflow.com/questions/4057199/animating-row-deletion-in-uitableview-with-coredata-gives-assertion-failure)




後來在Apple 範例程式找到一些線索（CoreDataBook)或是找[官方資料  
http://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/TableView_iPhone/Art/table_view_editing.jpg  
](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/TableView_iPhone/ManageInsertDeleteRow/ManageInsertDeleteRow.html#//apple_ref/doc/uid/TP40007451-CH10-SW9)




解決方式就是把deleteRowAtIndexPaths 由TableView觸發的event commitEditingStyle 搬移到 CoreData的delegate didChangeObject 
