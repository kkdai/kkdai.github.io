---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-02-27 00:51:47+00:00
slug: vs2013ci-%e9%97%9c%e6%96%bccommand-line-tool-msbuild-exe%e7%9a%84%e7%ad%86%e8%a8%98
title: '[VS2013][CI] 關於command line tool MSBuild.exe的筆記..'
wordpress_id: 1338
categories:
- CI_continuous integration
- NET程式設計
- 學習文件
---

 




這一些主要是為了想學習架設  [Jenkins](http://jenkins-ci.org/)，不過還沒整理完～先把這部分的整理出來....






  * **[問題] 利用command line 的 MSBuild.exe 會發生錯誤 “error MSB8020: The builds tools for v120 (Platform Toolset = 'v120') cannot be found.”**



    * 我也發現～就算你裝了 VS2013只要你有裝 VS2012 tool set你有可能會按倒 VS2012 command line tool，這時候的版本會出現  



      * msbuild.exe /version



        * 4.0.30319...




    * 解法: 首先要確認你執行是VS2013 command line tool或是執行 msbuild.exe 在  c:Program Fies(X86)MSBuild12.0Bin


    * 參考: [http://public.kitware.com/Bug/view.php?id=14369](http://public.kitware.com/Bug/view.php?id=14369)



  * **[問題]MSBuild去開始跑 *.sln 會出現 error MSB3779: The processor architecture of project being build "Any CPU” is not supported by referenced SDK ….**



    * 這個問題主要是因為在sln 裡面的設定 <PlatformTarget> 有加入當初建立project (專案)的時候的設定 Any CPU, Mixed Platform … etc.


    * 所以解法有以下的方式:


    * 解法:  [Build]->[Configuration Manager…] 去把沒需要build的platform 關閉，記得 “Build” 跟 “Deploy” 都要關閉．當然你真的都用不到～其實可以刪除...  


    * 參考: 



      * [http://stackoverflow.com/questions/1074654/how-do-i-force-msbuild-to-compile-for-32-bit-mode](http://stackoverflow.com/questions/1074654/how-do-i-force-msbuild-to-compile-for-32-bit-mode)


      * [http://stackoverflow.com/questions/3155492/how-to-specify-platform-for-msbuild](http://stackoverflow.com/questions/3155492/how-to-specify-platform-for-msbuild)




