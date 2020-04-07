---
layout: post
layout: post
author: kkdai
comments: true
date: 2005-09-04 17:39:26+00:00
slug: '%e5%a5%bd%e4%b8%8d%e5%ae%b9%e6%98%93%e9%87%8d%e6%96%b0%e7%81%8c%e5%9b%9e%e7%b3%bb%e7%b5%b1'
title: 好不容易重新灌回系統
wordpress_id: 256
categories:
- 柴米油鹽的東西
---

![](http://www.album.artfors.se/albums/album02/mandrake_star_o.sized.png)

好不容易，由於我每次只要出國在外，家裡的家人對於電腦的使用相當的不熟悉。每次不是給我亂重開機，就是直接拔掉電源。加上台灣也有天災~~ 使得我的主機又掛點一次了，硬碟就活生生給它死在那裡。不過還好的是~~ 可以及時將資料由舊的主機備份出來~ 不過也有些困難之處就這樣出來了~~~


<!-- more -->


**Mandrake 的預設ADSL連線在哪?**

要使用Mandrake 預設的ADSL連線(PPPOE)，就得在安裝的最後去設定網路設定裡麵添加ADSL設定，並且設為開機自動設定，如果你們跟我一樣需要NAT設定的話，使用Iptable可以輕易的做到NAT功能。

<blockquote>echo "1" > /proc/sys/net/ipv4/ip_forward  
modprobe ip_nat_ftp  
modprobe ip_nat_irc  
modprobe ip_conntrack  
modprobe ip_conntrack_ftp  
modprobe ip_conntrack_irc
> 
> iptables -t nat -A POSTROUTING -o ppp0  --source 192.168.8.1/32 -j MASQUERADE  
iptables -t nat -A POSTROUTING -o ppp0  --source 192.168.8.2/32 -j MASQUERADE  
iptables -t nat -A POSTROUTING -o ppp0  --source 192.168.8.3/32 -j MASQUERADE  
iptables -t nat -A POSTROUTING -o ppp0  --source 192.168.8.4/32 -j MASQUERADE  
iptables -t nat -A POSTROUTING -o ppp0  --source 192.168.8.5/32 -j MASQUERADE  
iptables -t nat -A POSTROUTING -o ppp0  --source 192.168.8.6/32 -j MASQUERADE  

> 
> </blockquote>

**MT 的Berkeley DB 掛點了?**

我相信許多人在安裝MT的時候，還是會使用Berkeley DB的選項來當MT的資料庫，由於它是內建的資料庫型態，加上複製容易，使用率是相當的普遍。雖然我自己也透過複製的方式來轉移過數次MT，但是這次忽然就給它不能開啟!! 

不能開啟??哪ㄋㄧ? 就是[這樣完全的開不起來](http://66.102.7.104/search?q=cache:AaO7xODSeRMJ:www.totalchoicehosting.com/forums/lofiversion/index.php/t20624.html+berkeley+db+transfer+mysql+MT&hl=zh-TW)，也無法去使用了~~當我跳腳到沒辦法的時候，忽然想起網路上的這篇文章[how to upgrade MT ](http://www.sixapart.com/movabletype/docs/mtupgrade)裡面曾經有提過ㄧ個將Berkeley DB轉換到MySQL的方式，但是~~~重要問題來了? 現在[要到哪裡去找MT的Upgrade](http://mtbook.net/dl-mt-2.661-full.html)程式(mt-db2sql.cgi?)阿??講真的Jedi大大所貼的圖的方式，相當的麻煩，除了要註冊一個你幾乎不相用到的帳號，還要找個半天，才能找到這支程式。所以重點([.](http://www.evanlin.com/blog/mt.upgrade.tgz))來了，只要將重點另存解壓縮後，相信可以幫助大家獲得所需要的那隻程式了~~~ 直接用這支程式可以將berkeley db轉換成MySQL內的資料，果然~~ 這樣就給它復活了~~ 

這些是我這次復活主機所發現的問題，不過我現在也設定好了自動備份機制

<blockquote>mysqldump --all-databases  > 輸出檔案
> 
> </blockquote>

這個方式可以將MySQL裡面的資料庫完整備份出來~~ 
