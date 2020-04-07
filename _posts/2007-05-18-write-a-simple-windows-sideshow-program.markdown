---
layout: post
layout: post
author: kkdai
comments: true
date: 2007-05-18 06:13:23+00:00
slug: write-a-simple-windows-sideshow-program
title: Write a simple Windows SideShow program
wordpress_id: 646
categories:
- NET程式設計
---

![筆記型電腦上的 Windows SideShow](http://www.microsoft.com/taiwan/windowsvista/images/features/feat_sideshow_1.jpg)

Windows SideShow is a new technology in Windows Vista that supports a secondary screen on your mobile PC. With this additional display you can view important information whether your laptop is on, off, or in sleep mode. Windows SideShow is available in Windows Vista Home Premium, Windows Vista Business, Windows Vista Enterprise, and Windows Vista Ultimate. For more detail, please refer [http://www.microsoft.com/windows/products/windowsvista/features/details/sideshow.mspx](http://www.microsoft.com/windows/products/windowsvista/features/details/sideshow.mspx)

For Engineer to development a simple SideShow program:

  1. Make sure you using Vista RTM 
  2. Download SideShow emulator: [http://www.microsoft.com/downloads/details.aspx?FamilyID=bba99eb2-aa6d-4133-b433-933a2c4d41dc&displaylang=en](http://www.microsoft.com/downloads/details.aspx?FamilyID=bba99eb2-aa6d-4133-b433-933a2c4d41dc&displaylang=en)
  3. Install VS2005. 
  4. Install Windows Vista SDK and .NET ([http://www.microsoft.com/downloads/details.aspx?FamilyId=C2B1E300-F358-4523-B479-F53D234CDCCF&displaylang=en](http://www.microsoft.com/downloads/details.aspx?FamilyId=C2B1E300-F358-4523-B479-F53D234CDCCF&displaylang=en)) 
  5. Open solution: C:Program FilesMicrosoft SDKsWindowsv6.0SampleswinuisideshowHelloWorldHelloWorld.sln 
  6. Build it 
  7. Run registery "HelloWorld.reg"
  8. Run bin file in C:Program FilesMicrosoft SDKsWindowsv6.0SampleswinuisideshowHelloWorldDebugWindowsSideShowHelloWorld.exe 
  9. Start Emulator, you should show "Hello World" here.~~~

![Windows SideShow 頁面](http://www.microsoft.com/taiwan/windowsvista/images/features/feat_sideshow_3.jpg)
