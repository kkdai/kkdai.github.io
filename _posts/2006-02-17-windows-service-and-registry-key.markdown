---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-02-17 07:03:24+00:00
slug: windows-service-and-registry-key
title: “Windows Service” and “Registry key”
wordpress_id: 340
categories:
- VC++程式設計
---

![WindowsService (WinCE).JPG](http://www.evanlin.com/blog/archives/20060217/WindowsService%20(WinCE).JPG)

There are something we should keep in mind when we tried to write a Windows Service. The most important is the "Registry Key".

In Windows programming, we usually use registry key to setting some value for difference user. Regstry key control become more and more in your code, and we also usually  read the specific user in "**HKEY_CURRENT_USER" **first then open generic "**HKEY_LOCAL_MACHINE" .**

Here comes the problem, According this article([Services and the Registry](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dllproc/base/automatically_starting_services.asp)) in [MSDN](http://msdn.microsoft.com/). We should not use "**HKEY_CURRENT_USER" **to retrival current user's registry key value. Because Windows Services always startup before user login. It may happen some error or loading the wrong setting profile. If you still insist on using the current user registry key setting, please refer "_[RegOpenCurrentUser](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/sysinfo/base/regopencurrentuser.asp)_". In my experience use "[_RegQueryValueEx_](http://msdn.microsoft.com/library/default.asp?url=/library/en-us/sysinfo/base/regqueryvalueex.asp)" with "**HKEY_CURRENT_USER"** will** **retrival the RootUser's setting. It make me confuse.

So the better way the use registry key in Windows Service is handle the "**HKEY_LOCAL_MACHINE" **rather than "**HKEY_CURRENT_USER".**
