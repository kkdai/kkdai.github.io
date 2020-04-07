---
layout: post
layout: post
author: kkdai
comments: true
date: 2008-01-10 14:34:42+00:00
slug: translate-your-perl-code-to-execute-file-in-win32
title: Translate your PERL code to execute file in Win32
wordpress_id: 798
categories:
- NET程式設計
- VC++程式設計
---

![](http://www.indigostar.com/images/perl2exe.gif)

 

[Perl2EXE](http://www.indigostar.com/perl2exe.htm) is a very useful program when you try to translate your PERL code to a execute file without PERL environment.

 

There should be noted if you happen the same problem.

 

  
  1. Active PERL need install:        
Although we try to use [Perl2EXE](http://www.indigostar.com/perl2exe.htm) to translate PERL to execute file. We still need PERL runtime environment in the compiler machine. (Don't need for running machine). 
   
  2. Make sure Active PERL version is pair with your [Perl2EXE](http://www.indigostar.com/perl2exe.htm)         
For example if you use [Perl2Exe V7.02 for Win32](http://www.indigostar.com/download/p2x-7.02-Win32.zip) , you must use Perl 5.8.0, Perl 5.6.1, Perl 5.6.0 and Perl 5.005 which Active PERL version is [ActivePerl Perl 5.8.0 Build 806](http://downloads.activestate.com/ActivePerl/Windows/5.8/ActivePerl-5.8.0.806-MSWin32-x86.msi). Or you may get error as follow: 
 

 

C:p2x-8.60-Win32>perl2exe.exe sample.pl      
Perl2Exe V8.60 Copyright (c) 1997-2005 IndigoSTAR Software       
Registered to evan:kkdai:20080010, ENT version       
Converting 'sample.pl' to sample.exe

 

C:p2x-8.60-Win32>

 

 

 

Which I use [Perl2EXE](http://www.indigostar.com/perl2exe.htm) V8.8 but I don't have Active Perl 5.88

 

After I switch to [Perl2EXE](http://www.indigostar.com/perl2exe.htm) V8.6 and using Active PERL 5.86. It work as follows:

 

 

C:p2x-8.60-Win32>perl2exe.exe sample.pl      
Perl2Exe V8.60 Copyright (c) 1997-2005 IndigoSTAR Software       
Registered to evan:kkdai:20080010, ENT version       
Converting 'sample.pl' to sample.exe 

 

C:p2x-8.60-Win32>
