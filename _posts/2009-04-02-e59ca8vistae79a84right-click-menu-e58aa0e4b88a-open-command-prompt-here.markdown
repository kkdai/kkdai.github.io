---
layout: post
layout: post
author: kkdai
comments: true
date: 2009-04-02 09:32:06+00:00
slug: '%e5%9c%a8vista%e7%9a%84right-click-menu-%e5%8a%a0%e4%b8%8a-open-command-prompt-here'
title: 在Vista的Right Click menu 加上 open command prompt Here
wordpress_id: 1042
categories:
- 下一代視窗系統心得
---

![Command.jpg](http://farm4.static.flickr.com/3454/3406039328_34a5a62c9c.jpg)

 

 

 

Here is registry file, copy it and save to file which name is “Add.reg”.

 

Double click “Add.reg”

 

<blockquote>  
> 
> Windows Registry Editor Version 5.00 
> 
>    
> 
> [HKEY_LOCAL_MACHINESOFTWAREClassesFoldershellCommand Prompt]       
@="Open Command Prompt Here"
> 
>    
> 
> [HKEY_LOCAL_MACHINESOFTWAREClassesFoldershellCommand Promptcommand]       
@="Cmd.exe /k pushd %L"
> 
> </blockquote>
