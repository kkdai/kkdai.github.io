---
layout: post
layout: post
author: kkdai
comments: true
date: 2004-12-30 12:09:11+00:00
slug: code-complete-8-defensive-programming%e5%bf%83%e5%be%971
title: '[Code Complete] 8: Defensive Programming心得(1)'
wordpress_id: 176
categories:
- 學習文件
---

[![Code Complete](http://images.amazon.com/images/P/1556154844.01._PE34_SCMZZZZZZZ_.gif)](http://www.amazon.com/gp/product/images/1556154844/ref=dp_primary-product-display_0/104-0369123-4595153?%5Fencoding=UTF8&n=283155&s=books)

再此寫下一些最近的讀書心得，可能太多無法一次全部寫完。但是有時間會繼續將其補其，有興趣的人再往下看吧!!


<!-- more -->
**1.何謂Defensive Programming?**

_Defensive Programming_一辭簡單明瞭，在此書章節的開頭也有提到，所謂的Defensive Programming並不是增強程式的防禦，這樣是沒有用的。有太多種未知的輸入與狀況可能會出現，如何在已知、未知的環境中，對於正確、錯誤的輸入的反應，才是真正的Defensive Programming。

**2.保護你的程式--對於不正確的輸入(Input)**

好的程式絕對不會產出不正確的垃圾(garbage)，不論是正確的、非法的輸入下都應該如此。所以一個程式的開始，對於參數的輸入，在處理之前都應該要有檢查的機制，避免處理過程中錯誤的產生。(在此書中有提到相關概念稱為 Preconditional)在此章節中，本書提到許多詳細的方式，就讓我稍微用一些方式將其整理一下:

_Check the values of all data from external source_

對所有外來的變數，都應該要有做些檢查，比如說輸入字串就要能預防"Escape() "的輸入；對於網頁來的內容，要能分辨是不是含有非法的字串；對於數值要做"boundary check"，如此一來可大大降低錯誤產生的機率。

_Check the values of all routine input parameter_

處理的函式中，若要傳遞參數到其他的函式前，也要將其參數做相關的檢查，如此一來可避免不必要的錯誤值加以傳遞。

_Design how to handle bad inputs_

OK，即使你抓到了錯誤的輸入，最重要的是，你要怎麼處理這樣的狀況，回傳預設值、回傳上一次正確值、直接改為正確值...許多的方式可以應用。

**3.Assertion的應用**

的確，本書對於assertion如何應用的方法算是相當的詳細，但是如果更仔細的使用(大量範例與CODE)，可以去看[Debugging Windows Programs](http://www.amazon.com/exec/obidos/ASIN/020170238X/qid=1104408676/sr=2-2/ref=pd_ka_b_2_2/104-0369123-4595153)裡面第三章對於assertion有相當的介紹，不過在此書還是有提到幾個比較重要的地方:

_Use error-handling code for conditions you expect to occurs; use assertions for conditions that should never occur._

也就是知道有可能發生的錯誤(輸入數值錯誤、找不到資訊、無法完成的函式)要有相關的錯誤處理方式，但是對於不應該、也不會發生的錯誤(指標尚未指定，參考點消失，COM被release掉)都應該要用assertion去加以標示出來，這一點是我認為本書再此章節中最重要的一個概念。

_Avoid putting excutable code into assertions_

在VC++中，assertion在release build中是不會執行的，所以儘量不要有類似以下的程式碼。

<blockquote>ASSERT(DoWorkReturnStatus());
> 
> </blockquote>

_Use assetion to document and verify precoditions and posconditions._

針對函式內參數的應用，可以利用assertion去做一些邏輯上的檢查，藉以作一份記錄。比如說，輸入的月份應該在1~12月之間，就可以寫成

<blockquote>ASSERT(nMonth > 0 && nMonth <= 12);
> 
> </blockquote>

_For high robus code, assert and then handle the error anyway._

在此，作者建議。避免無法預測的錯誤，先ASSERT在針對ASSER的狀況去處理(光是assertion並不能解決問題，還是得將錯誤作一個妥善的控制才對。)這樣一來工程師可以快速的找到錯誤可能發生的地方，而程式本身也不會被不可預知的錯誤破壞。
