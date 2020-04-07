---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-04-19 11:15:05+00:00
slug: string-compare-using-xslt-and-xpath
title: String compare using XSLT and XPATH
wordpress_id: 382
categories:
- VC++程式設計
- XML程式設計
- 學習文件
---

![](http://architag.com/xmlu/play/show/XSLServer.gif)

I have summerized some note about the string comparison via [XSLT](http://www.w3.org/TR/xslt) and [XPATH](http://www.w3.org/TR/xpath).

  1. **Find specific string with "="**:  
_Find specific Item with text "testITEM".  
_  
<xsl:apply-templates select="//*[Item[text() = 'testITEM']]">  
</xsl:apply-templates>  

  2. **Find specific string with "contains"**:  
_Find specific Item which contain "testITEM"._  
  
<xsl:apply-templates select="//*[contains(Item,'testITEM']">  
</xsl:apply-templates>  

  3. **Find specific string  case-insensitivity "="**:  
_Find specific Item with text "testITEM" or "TESTITEM" or "TestItem" ... ETC._  
  
There is no way to compare it without transform, you need transform it to upper case(or small case) first.  
  
<xsl:variable name="UperCase">ABCDEFGHIJKLMNOPQRSTUVWXYZ</xsl:variable>  
<xsl:variable name="LowerCase">abcdefghijklmnopqrstuvwxyz</xsl:variable>  
  
<xsl:apply-templates select="//*[Item[translate(text(), $UperCase, $LowerCase) = translate('testITEM', $UperCase, $LowerCase)]]">  
</xsl:apply-templates>  

  4. **Compare orderings with great-than**:  
_Find specific date which great than "20060419":  
_  
If the date is ISO format (YYYY-MM-DD) it is easy to compare with follow code. ( >= is the same case.)  
  
<xsl:apply-templates select="//*[date[translate(text(), '-', '') > translate('2006-04-19', '-', '')]]">  
</xsl:apply-templates>  

  5. **Compare orderings with less-than**:  
_Find specific date which less than "20060419":_  
  
It should be note, "<" can not in XPATH. Please use "&lt;" to replace it. "<=" is the same code.  
  
<xsl:apply-templates select="//*[date[translate(text(), '-', '') < translate('2006-04-19', '-', '')]]">  
</xsl:apply-templates>

That is all, for more detail you can find some useful feedback in [XSLT Questions and Answers](http://www.dpawson.co.uk/xsl/sect2/sect21.html).
