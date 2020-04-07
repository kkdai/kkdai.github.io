---
layout: post
title: "[Linux] Summary how to retreival system info"
description: ""
category: 
- Golang
tags: ["linux", "monitor"]

---


## Data Q&A

- How to get disk and network stat?
	- Try to usse `sar` ([detail usage](http://www.thegeekstuff.com/2011/03/sar-examples/?utm_source=feedburner))
- How could I get GPU info?
	- 	Try to use `glxinfo` ([detail usage](http://askubuntu.com/questions/5417/how-to-get-gpu-info))
-  How to get OS version?
	-  [Checking your Ubuntu Version](https://help.ubuntu.com/community/CheckingYourUbuntuVersion)

You also can get other detail on `/proc` ([detail spec](https://www.mjmwired.net/kernel/Documentation/filesystems/proc.txt)), such as:

- cpu
- memory
- process info (id, usage, ... other)
- DMA info




## Other Links

- [How to get GPU info?](http://askubuntu.com/questions/5417/how-to-get-gpu-info)
- [10 Useful Sar (Sysstat) Examples for UNIX / Linux Performance Monitoring](http://www.thegeekstuff.com/2011/03/sar-examples/?utm_source=feedburner)
- [Checking your Ubuntu Version](https://help.ubuntu.com/community/CheckingYourUbuntuVersion)
- [/proc spec](https://www.mjmwired.net/kernel/Documentation/filesystems/proc.txt)