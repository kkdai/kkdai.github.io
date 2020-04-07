---
layout: post
layout: post
author: kkdai
comments: true
date: 2011-11-16 11:49:55+00:00
slug: microsoft-windows-8-build-study-note-3-about-touch
title: MICROSOFT WINDOWS 8 BUILD STUDY NOTE (3) ABOUT touch
wordpress_id: 1136
categories:
- 學習文件
- 下一代視窗系統心得
---

![](http://files.channel9.msdn.com/thumbnail/ace11650-7aa0-4a31-8689-0890e91146e9.png)

**APP-186T - Build advanced touch apps in Windows 8**

refer: [http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-186T](http://channel9.msdn.com/Events/BUILD/BUILD2011/APP-186T)



	
  * Major point of touch app design:

	
    * Unify pen and touch into PointerPoint

	
    * Get mouse and pen for free

	
    * performance performance and performance




	
  * Touch and Gesture support in framework

	
    * Metro app with HTML or XAML—> Gesture event

	
    * ICoreWindow –> Using point (touch simulate mouse click)

	
    * Windows Runtime –> PointerPoint with GestureRecognizer




	
  * PointerPoint:

	
    * Whole related point implement is here, HID data also can get using PointerPoint.




	
  * GestureRecognizer:

	
    * It just like the Win7 multiple touch support on MSFT, it support kind of gesture such as:

	
      * Tap, Hold, rotate and Scale




	
    * But more support on Win8 such as:

	
      * HoldWithMouse?

	
      * RightTap

	
        * To simulate with right click on mouse, such your finger about right click here.

	
        * mmmm….. it should be MSFT patent here for right click.










	
  * For desktop developer, MSFT also support full Windows Runtime about Gesture support –> Great!

	
    * WM_Point –> the same as usual

	
    * InteractionContexts –> mirror GestureRecognizer. Don’t forget WM_TOUCH and WM_GESTURE still support here.

	
    * Pointer device –> identify source device (touch panel, mouse or other.)





