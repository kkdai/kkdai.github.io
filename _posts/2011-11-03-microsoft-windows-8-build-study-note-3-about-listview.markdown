---
layout: post
layout: post
author: kkdai
comments: true
date: 2011-11-03 11:41:29+00:00
slug: microsoft-windows-8-build-study-note-3-about-listview
title: MICROSOFT WINDOWS 8 BUILD STUDY NOTE (3) About ListVIEW
wordpress_id: 1134
categories:
- 學習文件
- 下一代視窗系統心得
---

![](http://files.channel9.msdn.com/thumbnail/ace11650-7aa0-4a31-8689-0890e91146e9.png)

APP-209T Build polished collection and list apps in HTML5

refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-209T](http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-209T)



	
  * Collection: A set of related user content.

	
  * This session talking about how to write customized list view by WinJS by uing

	
    * Data Source

	
    * Item Render (CSS)

	
    * Layout (WinJS.UI.GridLayout)




	
  * It could be easy to select image list view import your data source and put basicc control on your list view.

	
  * Multi-select already support just add one propoties on it. (selectionMode = 'multi’)

	
  * ListView is easy to grouping if you could provide detail data of each item, group rule and group render (to descript how to group).

	
  * Symantic Zoom:

	
    * It is a way to zoom out/zoom in by semantic  (such of “category”, “date”). It look like “Group By”.

	
    * ListView also provide semantic zoom.

	
    * Easy to implement to change <div id> to “zoomOutView” and related data control.




	
  * If app is snapped (snapped on side).


