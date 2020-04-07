---
layout: post
layout: post
author: kkdai
comments: true
date: 2004-12-08 13:59:09+00:00
slug: vc-%e9%97%9c%e6%96%bc%e4%bd%bf%e7%94%a8ole-drag-and-drop%e7%9a%84%e8%a3%9c%e5%85%85
title: VC++  關於使用OLE  Drag and Drop的補充
wordpress_id: 169
categories:
- VC++程式設計
---

[圖片來自[![The Code Project](http://www.thecodeproject.com/images/standard/logo225x72.gif)](http://www.codeproject.com/)]

![](http://www.codeproject.com/shell/ExplorerDragDrop/ExplorerDragDrop8.gif)

如果有人依據[How to Implement Drag and Drop Between Your Program and Explorer](http://www.codeproject.com/shell/explorerdragdrop.asp?select=502828&df=100&forumid=1699&exp=0)的文章自己將他的Class加入自己程式之中，或許會發現無法順利將CMyDropTarget 順利的在自己程式中Register，尋找一些書籍之後，我在[MFC (Programming Windows with MFC, 2/e)](http://www.tenlong.com.tw/BookSearch/Search.php?isbn=9861252983&sid=22020)中Putting It All Together: The Widget Application尋找出解決的方式

那是因為程式沒有對於OLE做起始化的設定，請在自己程式引用
    
    #include <afxole.h>
    

並且在::InitInstance()加入

 if (!AfxOleInit ()) {  
     AfxMessageBox (_T ("AfxOleInit failed, you may not use drag and drop"));  
  return FALSE;  
    }

  
就可以了~~~~
