---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-09-22 12:11:02+00:00
slug: becareful-to-uses_conversion
title: Becareful to “USES_CONVERSION”
wordpress_id: 266
categories:
- VC++程式設計
---

![MSDN Home](http://msdn.microsoft.com/msdn-online/shared/graphics/right_bnr_vstudio.jpg)

<blockquote>BSTR bstrMyBeaster = SysAllocString (L"Tring, Tring!");  
WCHAR* pwszMyWCharString = L"Tring, Tring!";  
  
USES_CONVERSION;  
LPSTR pszCharStringFromBSTR = OLE2A (bstrMyBeaster);  
LPSTR pszCharStringFromLPWSTR = W2A (pwszMyWCharString);  
// ...  
SysFreeString (bstrMyBeaster);
> 
> </blockquote>

I think "**USES_CONVERSION;**" is very convenient and easy to convert between _CComBSTR_ and _Char_. But [this article](http://www.codeguru.com/forum/archive/index.php/t-337247.html) also tell us it may happen "Stack overflow" when your source BSTR is over 1MB. The root cause is because use  ["**_alloca**"](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/vclib/html/_crt__alloca.asp).

I think to avoid stack overflow like this,  just use static conversion  not "USE_CONVERSION;".
