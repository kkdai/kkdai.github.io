---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-05-30 02:17:16+00:00
slug: '%e5%9c%a8c%e4%b8%8a%e4%bd%bf%e7%94%a8%e9%a1%9e%e4%bc%bcsprintf%e7%9a%84%e5%ad%97%e4%b8%b2%e8%bd%89%e6%8f%9b'
title: 在C#上使用類似sprintf的字串轉換
wordpress_id: 658
categories:
- NET程式設計
---

[![Solving Constraints](http://img.microsoft.com/taiwan/vstudio/vcsharp/images/NETbug.jpg)](http://msdn.microsoft.com/netframework/programming/classlibraries/)

**English Summary**: This article describe about how to use String.Format to implement the same with sprintf or printf in C++. For English, Please [refer this article for more detail](http://blog.stevex.net/index.php/string-formatting-in-csharp/).

**中文**: 最近遇到一些好玩的現象~~ 再次就把解決方式寫下來吧~~ (這篇就不用寫英文了，要看英文的部份請看[這篇文章](http://idunno.org/archive/2004/14/01/122.aspx))。

最近在C#上面，想要輸出固定2個位元小數點浮點數~ 為了這件事情~~ 覺得有點難搞~~ 由於傳下來的數字是Double 來儲存的，並且有可能是循環小數 (比如說: 10/3 = 3.333)。 如果~~ 你單純的使用Math.Round() 比如以下的範例:

<blockquote>Double buf = 3.33333333333333333;  
buf = Math.Round(buf , 2);
> 
> </blockquote>

這個時候~ 你還是會發現 buf 還是那一連串的小數點~ 而沒有再小數點第二位去取四捨五入。原因在於小數點數字過多~ 而產生了 exception 的發生。 這個時候我又想了一個方法，參考以下的code:

<blockquote>Double buf = 3.33333333333333333;  
buf = Math.Abs(buf*100);  
buf = bug/100;
> 
> </blockquote>

很奇怪的~~ 這樣也是無法轉換到兩個小數點~ 還會變成原來的數字~~ (難道是最佳化?) 難道我得使用[CallingConvention Enumeration](http://msdn2.microsoft.com/en-us/library/system.runtime.interopservices.callingconvention.aspx)  的方式，把sprintf 當作外在function call嗎?

<blockquote>Imports System
Imports Microsoft.VisualBasic  
Imports System.Runtime.InteropServices  
  
Public Class LibWrap  
' Visual Basic does not support varargs, so all arguments must be   
' explicitly defined. CallingConvention.Cdecl must be used since the stack   
' is cleaned up by the caller.   
' int printf( const char *format [, argument]... )  
  
<DllImport("msvcrt.dll", CallingConvention := CallingConvention.Cdecl)> _  
Overloads Shared Function printf ( _  
    format As String, i As Integer, d As Double) As Integer  
End Function  
  
<DllImport("msvcrt.dll", CallingConvention := CallingConvention.Cdecl)> _  
Overloads Shared Function printf ( _  
    format As String, i As Integer, s As String) As Integer  
End Function  
End Class 'LibWrap  
  
Public Class App  
    Public Shared Sub Main()  
        LibWrap.printf(ControlChars.CrLf + "Print params: %i %f", 99, _  
                       99.99)  
        LibWrap.printf(ControlChars.CrLf + "Print params: %i %s", 99, _  
                       "abcd")  
    End Sub 'Main  
End Class 'App  

> 
> </blockquote>

還好之後在網路上有找到相關的程式String.Format() 去做類似~ sprintf 的轉換.

<blockquote>Double buf = 3.33333333333333333;  
string outputStr = String.Format("{0:f}", buf); 
> 
> </blockquote>

要更多範例~~ 可以去[這邊查詢](http://idunno.org/archive/2004/14/01/122.aspx)。
