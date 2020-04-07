---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-01-24 11:30:57+00:00
slug: '%e5%ad%b8%e7%bf%92-raspberrypi-%e5%b5%8c%e5%85%a5%e5%bc%8f%e9%9b%bb%e8%85%a6%e7%a0%94%e7%bf%92%e7%87%9f-0124'
title: '[學習] RaspberryPi 嵌入式電腦研習營 01/24'
wordpress_id: 1301
categories:
- 研討會心得
- 嵌入系統程式設計
---

**參考資料:**






	
  * [http://www.raspberrypi.com.tw/](http://www.raspberrypi.com.tw/)

	
  * [http://www.raspberrypi.com.tw/faqs/](http://www.raspberrypi.com.tw/faqs/)

	
  * [https://github.com/raspberrypi](https://github.com/raspberrypi)

	
  * [http://www.element14.com/community/community/raspberry-pi?view=documents](http://www.element14.com/community/community/raspberry-pi?view=documents)

	
  * [http://www.raspberrypi.org/downloads](http://www.raspberrypi.org/downloads)

	
  * 電路圖 [http://www.raspberrypi.org/wp-content/uploads/2012/04/Raspberry-Pi-Schematics-R1.0.pdf](http://www.raspberrypi.org/wp-content/uploads/2012/04/Raspberry-Pi-Schematics-R1.0.pdf)

	
  * 講師: [http://yennan.blogspot.tw/](http://yennan.blogspot.tw/)

	
  * Raspberry Pi模擬器 QEMU [http://zh.wikipedia.org/wiki/QEMU](http://zh.wikipedia.org/wiki/QEMU)

	
  * RoR on Pi: [http://raspberrypi.stackexchange.com/questions/4597/setting-up-a-ruby-on-rails-server](http://raspberrypi.stackexchange.com/questions/4597/setting-up-a-ruby-on-rails-server)

	
  * 

	
    * [http://elinux.org/RPi_Ruby_on_Rails](http://elinux.org/RPi_Ruby_on_Rails)

	
    * [http://sirlagz.net/2013/02/14/raspbian-server-edition-version-2-3/](http://sirlagz.net/2013/02/14/raspbian-server-edition-version-2-3/)











* * *



**第一堂課:**










* * *



**
**






**What is Raspberry Pi:**











	
  * 只需要very low poet

	
  * 

	
    * Model#B 700ma(3.5W)

	
    * Model#A 500ma(2.5W)

	
    * USB only output 500ma




	
  * Pi —> Python as major programming language.




**其他的Embedded System Board相關資源:**











	
  * BeagleBoard 小獵犬   NT 6000 up

	
  * 

	
    * [http://cms.mcuapps.com/series/training-beagles/adopt-a-beagle.html](http://cms.mcuapps.com/series/training-beagles/adopt-a-beagle.html)







**About Hardware in Raspberry Pi (model#B)**











	
  * ARMv6 (ARM11)

	
  * GPU

	
  * 

	
    * OpenGL 2.0

	
    * 1080p 30fps H264/MPEG4




	
  * 512M main memory

	
  * TP2 could help to check if problem happen, check if it is 5V.

	
  * OS:

	
  * 

	
    * 官方推薦使用Raspbian (change from Debian)

	
    * 

	
      * [http://downloads.raspberrypi.org/raspbian/images/](http://downloads.raspberrypi.org/raspbian/images/)




	
    * Pidora.

	
    * Noobs 比較簡單～但是相當的站記憶體..

	
    * Android OS could not run full function on Raspberry Pi, not a chase in Windows.







**燒錄OS到Raspberry Pi SD卡**








	
  * Download image [http://www.raspberrypi.org/downloads](http://www.raspberrypi.org/downloads)

	
  * Download tool for Windows:

	
  * Mac 使用系統的功能就可以

	
  * 

	
    * 參考[http://computers.tutsplus.com/articles/how-to-flash-an-sd-card-for-raspberry-pi–mac-53600](http://computers.tutsplus.com/articles/how-to-flash-an-sd-card-for-raspberry-pi--mac-53600)

	
    * 

	
      * diskutil list




	
    * 必須先unmount

	
    * 

	
      * `diskutil unmountdisk` `/dev/disk2`




	
    * 直接用系統指令去燒錄img (記得檔案名稱要改)

	
    * 

	
      * `dd` `if``=2014-01-07-wheezy-raspbian.img of=``/dev/disk1` `bs=2m`




	
    * 燒好後記得把 #hdmi_force_hotplug=1 uncomment掉

	
    * 他會有兩個分割區，一個是FAT(看得到） 另一個需要用工具才能查看.











* * *



第二堂課










* * *






**Raspberry Pi開機/設定**










	
  * 開機流程:

	
  * 

	
    * GPU啟動，載入bootcode.bin

	
    * [bootcode.bin]啟動快取與記憶體 載入start.elf

	
    * [start.elf] 讀取config.txt cmdline.txt 劃分記憶體

	
    * 載入作業系統 kernel,img (畫面開始出來)




	
  * 開機選單後，先選取第一個選項讓OS分割區占滿整個記憶卡(原本只占2.8G)

	
  * 多語言選項

	
  * 

	
    * en_US.UTF-8 UTF8

	
    * zh_TW BIG5

	
    * Locale 記得選 en_US.UTF-8




	
  * 鍵盤選項記得改成”美式鍵盤”

	
  * 

	
    * Generic 105-key (Intel) PC

	
    * Other

	
    * English (US)




	
  * 超頻建議:

	
  * 

	
    * 到900Mhz其實還好～其他可能需要小心




	
  * 預設帳號:

	
  * 

	
    * ID: pi

	
    * PW: raspberry (可以修改)




	
  * 如果需要重新設定 sudo raspi-config











* * *



第三堂課













* * *






Unix 基本(只記錄忘記的)










	
  * cat /proc/cpuinfo

	
  * 

	
    * 如果要姐VCD或是其他需付費的影像，可以拿這個序號去購買license然後解鎖




	
  * Ctrl+Alt+F1~F6

	
  * 

	
    * 切換登入console ~可能需要，當你安裝相當大的套件時

	
    * [http://linux.vbird.org/linux_basic/0160startlinux.php](http://linux.vbird.org/linux_basic/0160startlinux.php)











* * *



GPIO










	
  * 需要先設定好每個腳位是輸出腳位還是輸入

	
  * 

	
    * sudo gpio -1 mode 11 out

	
    * sudo gpio -1 mode 13 in




	
  * 輸出電源

	
  * 

	
    * sudo gpio -1 write 11 1

	
    * sudo gpio -1 write 11 0

















* * *








XBMC











	
  * Mac 安裝法（不過裝了是網路image) [http://www.raspbmc.com/wiki/user/os-x-linux-installation/](http://www.raspbmc.com/wiki/user/os-x-linux-installation/)

	
  * 中文視訊檔案 [https://code.google.com/p/xbmc-addons-chinese/downloads/list](https://code.google.com/p/xbmc-addons-chinese/downloads/list)

	
  * [http://www.raspbmc.com/download/](http://www.raspbmc.com/download/)



