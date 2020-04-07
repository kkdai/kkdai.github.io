---
layout: post
layout: post
title: "[Visual Studio] Using Visual Studio 2015 to remote debugging C++ on linux"
description: ""
category: 
- VC++程式設計
tags: [vc, linux_programming]
---


![](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/65/69/2474.6-azurevm.png)

##Toturials 

[Announcing the VS GDB Debugger extension](https://blogs.msdn.microsoft.com/vcblog/2015/11/18/announcing-the-vs-gdb-debugger-extension/) or Chinese version Refer [here](http://blogs.msdn.com/b/ericsk/archive/2016/02/16/remote-gdb-debugging-c-and-cpp-program-via-vs-gdb-extension.aspx) to understand whole process.

## Note and Gotcha

#### How to add ssh key from Windows to Linux server?

- Use ssh-keygen.exe to generate SSH (SSH-2 RSA)
- Upload ssh public key to server:
	- login to server
	- `vi ~/.ssh/authorized_keys`
	- Add public key content to this file with start `ssh-rsa`
		- **Note: Don't forget the line break in windows, remove them all.**
	- Done
- Use private key locale to double confirm it.

#### My VS debugger cannot stop on breakpoint ?

Visual Studio will call gdb remotely and use gdb related command to communication. ex: add bookmark. 

So, your local file name must **identical** with the remote source file name.  
(ex: local: main.cpp, remote: main.cpp)

If file name not identical, the debugger will not stop on your breakpoint.


#### Where is pscp.exe and plink.exe

Download them from [putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) page

## Refer Link:

- [ "使用 Visual Studio GDB 擴充套件在 Visual Studio 上遠端偵錯 Linux 上的 C/C++ 程式"](http://blogs.msdn.com/b/ericsk/archive/2016/02/16/remote-gdb-debugging-c-and-cpp-program-via-vs-gdb-extension.aspx)
- [Announcing the VS GDB Debugger extension](https://blogs.msdn.microsoft.com/vcblog/2015/11/18/announcing-the-vs-gdb-debugger-extension/)
