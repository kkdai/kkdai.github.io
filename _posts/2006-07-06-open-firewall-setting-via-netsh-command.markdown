---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-07-06 10:06:39+00:00
slug: open-firewall-setting-via-netsh-command
title: Open firewall setting via netsh command
wordpress_id: 434
categories:
- VC++程式設計
---

![Firewall_Setting.JPG](http://www.evanlin.com/blog/archives/20060706/Firewall_Setting.JPG)

From XP-SP2. the firewall setting default will block all app connection except user approval. So, you can approve every app you want to execute and let it work correctly under XP-SP2 (also in Vista). 

_But if your appllication is a Window Services? How it work under Microsoft firewall ?_

It will be blocked the communication automatically!!

Yes, ~~  if your app is a P2P window services, you will find it doesn't work when the service is work, even you never get a assert dialog box.

![BlockAllowed.JPG](http://www.evanlin.com/blog/archives/20060706/BlockAllowed.JPG)

_How do workaround with firewall, if my application is a Window Service?_

Use "[Netsh](http://support.microsoft.com/kb/875357/)" to change your firewall setting. For example if you want to enable specific app "fooAPP.exe".

_netsh firewall add allowedprogram "c:fooAPPfooAPP.exe" fooAPP ENABLE_

if you want to disable a APP from firewall.

_netsh firewall delete allowedprogram "c:fooAPPfooAPP.exe"_
