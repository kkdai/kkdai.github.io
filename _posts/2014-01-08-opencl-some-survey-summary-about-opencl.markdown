---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-01-08 15:15:26+00:00
slug: opencl-some-survey-summary-about-opencl
title: '[OpenCL] Some survey summary about OpenCL'
wordpress_id: 1286
categories:
- VC++程式設計
- 學習文件
- 下一代視窗系統心得
---






  * Related code survey found:



    * Debugging in Intel is easy refer [Using the Intel® OpenCL SDK Debugger](///view/16208274/s151/396127c3-36f6-4151-9c0d-74a15429a179/396127c3-36f6-4151-9c0d-74a15429a179/)



      * Add file path when you build cl program



        * Note: Must using original path not copy path.



          * EX: If you copy your CL file in post build process, need add original path not debugging CL code path.



        * Note: Path need full path



      * Enable Intel OpenCL SDK debugging in toolIntel SDK



        * Note: Work item set 0,0,0 as default is enough.





  * OpenCL kernel need warm up



    * Run any other kernel code first (even not the same application), it will speed up your major CL kernel code


    * AMD’s magic number is to run “twice” on dump kernel



      * Testing result: (SW/CPU 160ms)



        * Intel:



          * 1st time setup 700ms, effect 160



        * AMD:



          * 1st time



            * setup 6000ms


            * effect 16ms



          * 2nd time



            * setup 160ms


            * effect 16ms  (it might goes to 0ms some time)





      * It could be reduce time to pre-load *.cl file but no way to not reduce clBuildProgram. (Program will cache result as previous one, even you reset twice the argument.


      * Run two times the same kernel code:



        * After second time, (no matter the same program or not) the CL kernel will cache it and very fast.


        * Run second time, the CL setup code will faster the first time (Found on **AMD GPU**)


        * **Note: IVB don’t have such issue.**





  * Refer: 



    * Basic concept of OpenCL and sample program [OpenCL 教學（一） - Hotball's Hive](///view/16208274/s151/0bfa0606-ce5c-423d-8c25-90aebb86fea1/0bfa0606-ce5c-423d-8c25-90aebb86fea1/)


    * Advanced concept and clarify OpenCL comment [Richard's blog: OpenCL 介紹](///view/16208274/s151/5cda875c-cfbc-40b0-8254-6c07cd6be7bf/5cda875c-cfbc-40b0-8254-6c07cd6be7bf/)



