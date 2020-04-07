---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-04-09 20:22:26+00:00
slug: some-interest-thing-about-uac
title: Some interest thing about UAC
wordpress_id: 621
categories:
- 下一代視窗系統心得
---

<table class="dtTABLE" ><tbody ><tr valign="top" >
<td width="31%" >

![Windows Vista](http://www.microsoft.com/library/media/1033/windows/images/products/windowsvista/quick_vista.gif)

There are some interesting thing which I tried to understand the [different UAC behavior with Vista verion #5XXX with RTM version](http://technet.microsoft.com/en-us/windowsvista/aa905117.aspx). 

</td></tr></tbody></table>

  1. **How to not store file in virtual store when UAC is turn on?  
**_Ans_: Use trust info in your manifest info. It can announce UAC not to use [virtual store](http://technet.microsoft.com/en-us/windowsvista/aa905117.aspx)([Virtual Store Redirection ](http://techtalkblogs.com/blog/archive/2006/11/11/774.aspx)was implemented to maximise compatibility of applications moving into the more secure Windows Vista environment without the need to recompile application code).  
  
What if you don't want to save file in virtual store when your privilege is not administrator? (Just let don't let user to write file in such C:Program Files; C:; C:Windows ... etc)  
  
If you add follow code in you manifest, virtual store will be disable.  
<security>  
 <requestedPrivileges>  
  <requestedExecutionLevel level="asInvoker" uiAccess="false"/>  
 </requestedPrivileges>  
</security>  
  
For this case, you should add trust infomation in your manifest file which like ([Require Administrator](http://notgartner.com/Downloads/RequireAdministrator.res)/[As Invoker](http://notgartner.com/Downloads/AsInvoker.res)). You can refer[ this article for more detail](http://techtalkblogs.com/blog/archive/2006/11/11/774.aspx).   
  

  2. **Don't use MT.exe to link manifest resource in your .exe and .dll  
**_Ans_: As I found trust information about manifest file in [Microsoft Forum](http://msdn2.microsoft.com/en-us/default.aspx).  This [discussion thread also suggest us not to use MT.exe to link manifest information in your .exe and .dll](http://forums.microsoft.com/MSDN/ShowPost.aspx?PostID=1188754&SiteID=1), since there might a bug from Microsoft which will cause Blue screen (Yes,!!  Blue God damn screen.) or freeze in your computer.   
  
It will happen when your manifest information contain   
  
_<ms_asmv3:trustInfo xmlns:ms_asmv3="urn:schemas-microsoft-com:asm.v3">  
  
_For more information, you can refer this [article(The computer may restart when you add a manifest that has the Windows Vista extension to an .exe file or to a .dll file in Windows XP Service Pack 2 (SP2))](http://support.microsoft.com/kb/921337/en-us). It is better to use [ThemeMe.exe](http://www.msjogren.net/dotnet/eng/tools/default.asp)  to link it.  
  

  3. **Does UAC really safe for us?  
**_Ans_: There is also an interesting news which [Symantec: Don't Trust Windows Vista UAC Prompts!](http://news.softpedia.com/news/Symantec-Don-039-t-Trust-Windows-Vista-UAC-Prompts-47569.shtml). This news show, Symantec found Microsoft can use RunLegacyCPLElevated.exe(Which is designed to provide backward compatibility by allowing legacy Windows Control Panel plug-ins to run with full administrative privileges.) to get full  administration privilege to call interface "CPlApplet", which is then called with a number of different parameters depending on the action being performed.
