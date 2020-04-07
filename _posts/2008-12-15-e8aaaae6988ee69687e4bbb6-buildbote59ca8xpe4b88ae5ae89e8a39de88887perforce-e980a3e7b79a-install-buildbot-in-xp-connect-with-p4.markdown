---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-12-15 15:10:27+00:00
slug: '%e8%aa%aa%e6%98%8e%e6%96%87%e4%bb%b6-buildbot%e5%9c%a8xp%e4%b8%8a%e5%ae%89%e8%a3%9d%e8%88%87perforce-%e9%80%a3%e7%b7%9a-install-buildbot-in-xp-connect-with-p4'
title: 說明文件–Buildbot在XP上安裝與Perforce 連線 (Install buildbot in xP connect with P4)
wordpress_id: 1002
categories:
- NET程式設計
- 學習文件
---

![Buildbot.jpg](http://farm4.static.flickr.com/3065/3109202311_a28bfc7ed9.jpg)

 

要轉錄請務必標示文章出處: BlogE [www.evanlin.com/blog](http://www.evanlin.com/blog)

 

**甚麼是BuildBot?**

 

[Buildbot](buildbot.net) 是一套由Python撰寫的client-server 架構的專案source code追蹤用的伺服器。 主要可以拿來做以下的用途:

 

  
  * **Code quality tracing**: 可以定期或是只要有新的變動就主動下載source code並且build。 可以查出來哪個變動讓程式build不過。         

   
  * **Multiple platform compiler testing:** 針對一份程式碼要對應到多個platform 或是多整module, [Buildbot](buildbot.net) 提供了很好的方式可以幫助你確認這件事情。         

   
  * **Unit testing**: 使用[Buildbot](buildbot.net) 可以定期幫你的程式做unit testing。(當然~ unit testing program 得是你自己有準備好的)         

 

**需要的軟體:**

 

在這裡先介紹所有你需要的軟體，可以事先去download 下來。

 

  
  * Python 2.44 Win32 install binary [python-2.4.4.msi](http://www.python.org/ftp/python/2.4.4/python-2.4.4.msi)
   
  * Install Zope 3.01 final [zope.interface-3.0.1.win32-py2.4.exe](http://www.zope.org/Products/ZopeInterface/3.0.1final/zope.interface-3.0.1.win32-py2.4.exe)
   
  * Install Pywin32 extension [pywin32-212.win32-py2.4.exe-modtime=1217535935&big_mirror=0&filesize=5409684](http://downloads.sourceforge.net/pywin32/pywin32-212.win32-py2.4.exe?modtime=1217535935&big_mirror=0&filesize=5409684)
   
  * Install twisted -- 2.0 [Twisted_NoDocs-8.1.0.win32-py2.4.exe](http://tmrc.mit.edu/mirror/twisted/Twisted/8.1/Twisted_NoDocs-8.1.0.win32-py2.4.exe)
   
  * Buildbot (0,79) [downloading.php-groupname=buildbot&filename=buildbot-0.7.9.zip&use_mirror=nchc](http://sourceforge.net/project/downloading.php?groupname=buildbot&filename=buildbot-0.7.9.zip&use_mirror=nchc)
   
  * MFC library for python [mfc71.dll](http://starship.python.net/crew/mhammond/downloads/mfc71.dll)
 

以上的版本已經確定在XP SP2 可以正確安裝並且執行無誤。希望各位也去下載適當版本。必須注意如果把Python換成新的版本(2.5 or 2.6)反而可能會造成安裝失敗。

 

或者你也可以去以下位置下載懶人包 。

 

懶人包: [http://www.badongo.com/cn/file/12490044](http://www.badongo.com/cn/file/12490044)

 

 

**安裝流程: **

 

  
  * Install python 2.44 binary [http://www.python.org/ftp/python/2.4.4/python-2.4.4.msi](http://www.python.org/ftp/python/2.4.4/python-2.4.4.msi)              
    1. Check to make sure your PATHEXT environment variable has ";.PY" in it -- if not set your global environment to include it. 
       
    2. I also modified PATH (in the same place as PATHEXT) to include C:Python24 and C:Python24Scripts . This will allow 'python' and (eventually) 'trial' to work in a regular command shell. 
       
   
  * Install Zope 3.01 final [http://www.zope.org/Products/ZopeInterface/3.0.1final/zope.interface-3.0.1.win32-py2.4.exe](http://www.zope.org/Products/ZopeInterface/3.0.1final/zope.interface-3.0.1.win32-py2.4.exe)
   
  * Install Pywin32 extension [http://downloads.sourceforge.net/pywin32/pywin32-212.win32-py2.4.exe?modtime=1217535935&big_mirror=0&filesize=5409684](http://downloads.sourceforge.net/pywin32/pywin32-212.win32-py2.4.exe?modtime=1217535935&big_mirror=0&filesize=5409684)              
    1. The installer complains about a missing DLL. Download mfc71.dll from the site mentioned in the warning 
       
    2. ([http://starship.python.net/crew/mhammond/win32/](http://starship.python.net/crew/mhammond/win32/)) and move it into c:Python24DLLs ([http://starship.python.net/crew/mhammond/downloads/mfc71.dll](http://starship.python.net/crew/mhammond/downloads/mfc71.dll) ) 
       
   
  * Install twisted -- 2.0 [http://twistedmatrix.com](http://twistedmatrix.com) . [http://tmrc.mit.edu/mirror/twisted/Twisted/8.1/Twisted_NoDocs-8.1.0.win32-py2.4.exe](http://tmrc.mit.edu/mirror/twisted/Twisted/8.1/Twisted_NoDocs-8.1.0.win32-py2.4.exe)
   
  * Download Buildbot [http://sourceforge.net/project/downloading.php?groupname=buildbot&filename=buildbot-0.7.9.zip&use_mirror=nchc](http://sourceforge.net/project/downloading.php?groupname=buildbot&filename=buildbot-0.7.9.zip&use_mirror=nchc)
   
  * Command lone on buildbot path, run "python setup.pyinstall". 
   
  * Change buildbot.bat from C:Python24Scripts to modify to @python C:Python24Scriptsbuildbot %* 
   
  * Check "buildbot --version" if show version correct it mean install finish 
 

接下來你還必須要設定master.cfg 才能正常的啟動。

 

**設定Buildbot:**

 
* Create MASTER    

    
  1. "buildbot create-master <master_path>" <master_path> should be a path for Buildbot using. For my case using "buildbot create-master MASTER". 
     
  2. Modify master.cfg on the <master_path>. (you could refer as master.cfg as lastest of this article) 
  
 
* Create Slave    

    
  1. buildbot create-slave bot1name bot1:8010 bot1 123(default Master port is the same with setting in Master.cfg)                 
    1. c['slavePortnum'] = 8010 #default is 9989 
           
  
 
* Start Master    

    
  1. "buildbot start MASTER"
  
 
* Start SLAVE    

    
  1. "buildbot start bot1"
      

You could connect to [http://localhost:10000](http://localhost:10000) to see the server status.

   

**Master.cfg**

   

<blockquote>    
>     
>     # 'master.cfg' in your buildmaster's base directory (although the filename
>     # can be changed with the --basedir option to 'mktap buildbot master').
>     
>     p4env = {
>     	'port': '192.168.XX.XX:XXXX',    #P4 connect port
>     	'user': 'user',
>     	'passwd': 'passwd',
>     }
>     
>     # This is the dictionary that the buildmaster pays attention to. We also use
>     # a shorter alias to save typing.
>     c = BuildmasterConfig = {}
>     
>     ####### BUILDSLAVES
>     
>     # the 'slaves' list defines the set of allowable buildslaves. Each element is
>     # a tuple of bot-name and bot-password. These correspond to values given to
>     # the buildslave's mktap invocation.
>     from buildbot.buildslave import BuildSlave
>     c['slaves'] = [BuildSlave("bot1", "123")]
>     
>     
>     # 'slavePortnum' defines the TCP port to listen on. This must match the value
>     # configured into the buildslaves (with their --master option)
>     
>     c['slavePortnum'] = 8010
>     
>     ####### CHANGESOURCES
>     
>     # the 'change_source' setting tells the buildmaster how it should find out
>     # about source code changes. Any class which implements IChangeSource can be
>     # put here: there are several in buildbot/changes/*.py to choose from.
>     
>     from buildbot.changes.p4poller import P4Source
>     c['change_source'] = P4Source(
>     	                      p4base='//depot/Shared/Util/P4ReleaseNote/',
>     	                      p4port = p4env['port'],
>     	                      p4user = p4env['user'],
>     	                      p4passwd = p4env['passwd'],
>     	                      pollinterval= 60,
>     	                      p4bin = 'c:/p4.exe',
>     	                      split_file=lambda branchfile: branchfile.split('/',1),)
>     
>     #from buildbot.changes.pb import PBChangeSource
>     #c['change_source'] = PBChangeSource()
>     
>     ####### SCHEDULERS
>     ## configure the Schedulers
>     
>     #Periodic scheduler<br></br>from buildbot.scheduler import Scheduler, Periodic
>     periodic = Periodic("every_1_minutes", ["main"], 60)
>     c['schedulers'] = [periodic]
>     
>     ####### BUILDERS
>     from buildbot.process import factory
>     from buildbot.steps.source import P4 #Must include P4 library.
>     from buildbot.steps.shell import Configure,Compile,Test
>     from buildbot.steps.python_twisted import Trial
>     
>     #######		Builder for P4  #############
>     f1 = factory.BuildFactory()
>     f1.addStep(P4,
>     	p4port = p4env['port'],
>     	p4user = p4env['user'],
>     	p4passwd = p4env['passwd'],
>     	p4client = 'BuildBot',
>     	p4base = '//depot/Shared/Util/P4ReleaseNote/',
>     	mode = "copy")
>     
>     b1 = {
>     	'name': "main",
>     	'slavename': "bot1",
>     	'builddir': "main",
>     	'factory': f1
>     }
>     
>     c['builders'] = [b1]
>     
>     
>     ####### STATUS TARGETS
>     c['status'] = []
>     
>     from buildbot.status import html
>     c['status'].append(html.WebStatus(http_port=10000))
>     
>     from buildbot.status import mail
>     c['status'].append(mail.MailNotifier(fromaddr="evan.lin@evanlin.com",
>                                          extraRecipients=["builds@example.com"],
>                                          sendToInterestedUsers=False))
>     ####### PROJECT IDENTITY
>     c['projectName'] = "Buildbot"
>     c['projectURL'] = "http://buildbot.sourceforge.net/"
>     c['buildbotURL'] = "http://localhost:10000/"
> 
> 
  </blockquote>



  

**後記:**



  

總算安裝跟設定好了吧~~~但是這樣僅僅只是讓你下載而已～～關於定期build的部分~ 我會研究出來後再分享給各位。 這裡有一些東西一定要注意到的是。 Perforce client spec must sync with your buildbot path。 如果沒有一樣的話，會發生下載下來檔案在原來client space 的地方，而不會在你預先設定的位置。



  

**Reference link** (參考資料)



  


    
  1. Patrick: Buildbot + p4 + windows [http://www.scribd.com/doc/4808056/buildbot-p4-windows](http://www.scribd.com/doc/4808056/buildbot-p4-windows)


    
  2. Buildbot manual [http://buildbot.net/repos/release/docs/buildbot.html](http://buildbot.net/repos/release/docs/buildbot.html)


    
  3. buildbot安装配置笔记 [http://blog.chinaunix.net/u2/68938/showart_1076484.html](http://blog.chinaunix.net/u2/68938/showart_1076484.html)


    
  4. buildbot系统维护知识 [http://blog.chinaunix.net/u2/68938/showart_1111019.html](http://blog.chinaunix.net/u2/68938/showart_1111019.html)


    
  5. buildbot setup trac! (Buildbot for WIKI) [http://timchen119.blogspot.com/2007/02/buildbot-setup-trac.html](http://timchen119.blogspot.com/2007/02/buildbot-setup-trac.html)


    
  6. How to build Mozilla with Buildbot [http://zenit.senecac.on.ca/wiki/index.php/Building_Mozilla_with_Buildbot](http://zenit.senecac.on.ca/wiki/index.php/Building_Mozilla_with_Buildbot)


    
  7. User:Bhearsum:Build/Buildserver ref image [https://wiki.mozilla.org/User:Bhearsum:Build/Buildserver_ref_image](https://wiki.mozilla.org/User:Bhearsum:Build/Buildserver_ref_image)


    
  8. Development: Buildbot [http://wiki.wxwidgets.org/Development:_Buildbot](http://wiki.wxwidgets.org/Development:_Buildbot) (Wiki page with full doc for buildbot) 

  

