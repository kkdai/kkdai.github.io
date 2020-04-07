---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-06-08 04:05:19+00:00
slug: how-to-use-key-in-xslt
title: How to use KEY in xslt
wordpress_id: 410
categories:
- VC++程式設計
- XML程式設計
---

[![W3C](http://www.w3.org/Icons/WWW/w3c_home)](http://www.w3.org/)

I think XML is a very simple storage medium that you can save and query in it. But all data is storage as a tree node in the XML. In this case, it can very simple to save hierarchy data structure. But how to handle it you have reference item? 

Just use key in your XML code. For more detail please refer the sample as follow:

t1.xslt

<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:key name="ownerById" match="item" use="@ID"/>
  <xsl:output indent="yes"/>
  <xsl:template match="RefItem">
<xsl:text>
---------------------------------------------------------------------------------
                                 Result
---------------------------------------------------------------------------------
</xsl:text>
    <xsl:for-each select="item">
<xsl:if test="key('ownerById', @refID)//name">
<xsl:text disable-output-escaping="yes">
 &lt;item ID="</xsl:text>

<xsl:value-of select="@ID"/>

<xsl:text disable-output-escaping="yes">" refID="</xsl:text>

<xsl:value-of select="@refID"/>

<xsl:text disable-output-escaping="yes">">
  &lt;name></xsl:text>

<xsl:value-of select="key('ownerById', @refID)//name"/>

<xsl:text disable-output-escaping="yes">&lt;/name>
  &lt;adress></xsl:text>
<xsl:value-of select="key('ownerById', @refID)//adress"/>

<xsl:text disable-output-escaping="yes">&lt;/adress>
&lt;/item>
</xsl:text>
</xsl:if>

  </xsl:for-each>
  </xsl:template>
  <xsl:template match="@* | node()">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()"/>
    </xsl:copy>
  </xsl:template>
</xsl:stylesheet>

x1.xml

<test>
  <item ID ="100">
    <name>J. Smith</name>
    <adress>Taipei, Taiwan</adress>
  </item>
  <!--
   <item ID ="110">
    <name>E. Rock</name>
    <adress>CA, US</adress>
  </item>
  -->
  <item ID ="120">
    <name>R. Paul</name>
    <adress>TAIWAN, SA</adress>
  </item>
  <item ID ="130">
    <name>E. Rock</name>
    <adress>CA, US</adress>
  </item>
  <RefItem>
    <item ID ="151" refID="130"/>
    <item ID ="152" refID="120"/>
    <item ID ="153" refID="130"/>
    <item ID ="154" refID="110"/>
    <item ID ="155" refID="100"/>
  </RefItem>
</test>

Result

<?xml version="1.0" encoding="utf-8"?>
<test>
  <item ID="100">
    <name>J. Smith</name>
    <adress>Taipei, Taiwan</adress>
  </item>
  <!--
   <item ID ="110">
    <name>E. Rock</name>
    <adress>CA, US</adress>
  </item>
  -->
  <item ID="120">
    <name>R. Paul</name>
    <adress>TAIWAN, SA</adress>
  </item>
  <item ID="130">
    <name>E. Rock</name>
    <adress>CA, US</adress>
  </item>

---------------------------------------------------------------------------------
                                 Result
---------------------------------------------------------------------------------

 <item ID="151" refID="130">
  <name>E. Rock</name>
  <adress>CA, US</adress>
</item>

 <item ID="152" refID="120">
  <name>R. Paul</name>
  <adress>TAIWAN, SA</adress>
</item>

 <item ID="153" refID="130">
  <name>E. Rock</name>
  <adress>CA, US</adress>
</item>

 <item ID="155" refID="100">
  <name>J. Smith</name>
  <adress>Taipei, Taiwan</adress>
</item>

</test>
