---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-04-18 12:56:45+00:00
slug: codehighlighter-nice-tool-to-display-code-in-web-page
title: CodeHighlighter — Nice tool to display code in web page
wordpress_id: 230
categories:
- 網路上好玩的事情
---

![Designing .NET Class Libraries: API Usability](http://msdn.microsoft.com/netframework/art/NETFw.jpg)由於我經常去[博客網](http://blog.joycode.com/)看一些高手的文章，也常常看到他們利用工具將自己的程式碼格式化後在貼上來，整個的規格相當整齊，真是令我羨慕~~還好現在找到了[CodeHighlighter ](http://www.actiprosoftware.com/Products/DotNet/CodeHighlighter/PasteCode.aspx)。

要完整安裝使用這套服務，不僅僅要將轉換出來的程式碼複製過來，還要事先將數個檔案複製過來，貼在自己目錄下的  /Images/OutliningIndicators/ ，要有哪些檔案呢？　就先複製以下的這些檔案吧。 [完整檔案](http://www.evanlin.com/Images/AllFile.tat.gz)


<!-- more -->


1![](/Images/OutliningIndicators/None.gif)namespace ConsoleApplication2
2![](/Images/OutliningIndicators/ExpandedBlockStart.gif)![](/Images/OutliningIndicators/ContractedBlock.gif)...{
3![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)    /**//// <summary>
4![](/Images/OutliningIndicators/InBlock.gif)    /// Summary description for Class1.
5![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)    /// </summary>
6![](/Images/OutliningIndicators/InBlock.gif)    class Class1
7![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)    ...{
8![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)        /**//// <summary>
9![](/Images/OutliningIndicators/InBlock.gif)        /// The main entry point for the application.
10![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)        /// </summary>
11![](/Images/OutliningIndicators/InBlock.gif)        [STAThread]
12![](/Images/OutliningIndicators/InBlock.gif)        static void Main(string[] args)
13![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)        ...{
14![](/Images/OutliningIndicators/InBlock.gif)            int i = 0;
15![](/Images/OutliningIndicators/InBlock.gif)
16![](/Images/OutliningIndicators/InBlock.gif)            try
17![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)            ...{
18![](/Images/OutliningIndicators/InBlock.gif)                throw new FileNotFoundException();
19![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)            }
20![](/Images/OutliningIndicators/InBlock.gif)            catch(FileNotFoundException e)
21![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)            ...{
22![](/Images/OutliningIndicators/InBlock.gif)                //FileNotFoundException e
23![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)            }
24![](/Images/OutliningIndicators/InBlock.gif)            catch(Exception e)
25![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)            ...{
26![](/Images/OutliningIndicators/InBlock.gif)                //Exception e
27![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)            }
28![](/Images/OutliningIndicators/InBlock.gif)            finally
29![](/Images/OutliningIndicators/ExpandedSubBlockStart.gif)![](/Images/OutliningIndicators/ContractedSubBlock.gif)            ...{
30![](/Images/OutliningIndicators/InBlock.gif)                //finally
31![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)            }
32![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)        }
33![](/Images/OutliningIndicators/ExpandedSubBlockEnd.gif)    }
34![](/Images/OutliningIndicators/ExpandedBlockEnd.gif)}
35![](/Images/OutliningIndicators/None.gif)
