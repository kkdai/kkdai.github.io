---
layout: post
layout: post
title: "[Docker] 再來玩玩Docker，在Windows上遇到的困難與筆記"
description: ""
category: 
- docker
tags: []

---

![image](https://docs.docker.com/dist/assets/images/logo.png)

##前言

其實Docker一直是我一個很想玩但是玩不起的技術．主要是因為我的MBA空間太小，隨便一個docker image 就有3~5G．小小128G MBA實在無法來順利成行． 最近有要跨平台跑東西的需求，決定弄幾個docker image可以幫助我處理一些令人討厭的設定．  

這次主要是用到Windows桌機，裡面至少有足夠的硬碟空間可以讓我做這件事情．


## Docker Windows 可能遇到的困難與筆記

####Docker Quickstart Terminal的console 顯示實在令人搖頭

Windows 上的Docker Quicktstart Terminal 使用MingW Console本來也是還好，但是在我電腦實在就有console顯示的問題．  建議使用`ssh login`來跑．

方法如下:

- 跑"Docker Quickstart Terminal" 來將docker VM 跑起來        
- 你會看到 `"default" machine with IP 192.168.99.100` (每個人IP不同)
- 使用[putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)來登入SSH  `putty.exe -ssh docker@192.168.99.100:2376 -pw tcuser` 帳號跟密碼都是docker預設，沒有改過應該就可以正常跑．

把以上的指令弄成捷徑在桌面，就可以登入數個docker console


#### Docker Quickstart Terminal console 會遇到的 volume mount問題

`Docker -v`提供一個很好的方式可以讓本地端的檔案跟docker container去對應與操作．但是在Windows上面會遇到永遠找不到的路徑問題．（詳細可以參考[這裡](https://github.com/docker/docker/issues/12751) 還有[這個](https://github.com/boot2docker/boot2docker/issues/846)) 

        ##這樣在Windows boot2docker 跟 docker quickstart terminal 都會失敗
        docker run -v /c/Users/USER/TEST:/OUTPUT --rm=true DOCKER_IMAGE_NAME
        
        
        ##必須使用以下的指令  注意是"//c"  而非 "/c"
        docker run -v //c/Users/USER/TEST:/OUTPUT --rm=true DOCKER_IMAGE_NAME
             
        
這個問題在我剛剛提到使用SSH console是不會有這樣的問題的，所以我`強烈建議`各位使用docker SSH console．


## 建立Dockerfile     

想要建立一個[Docker Hub](https://hub.docker.com/)可以讓人家可以下載的Image或是 Dockerfile，其實不難．步驟以下:

- 先建立一個github repository裡面只需要有Dockerfile (當然有README.md 更好 :) )
- [Docker Hub](https://hub.docker.com/) (記得要先註冊帳號)  的 Dashboard 選取 [Create] -> [Create Automated Build]
- 輸入Github帳號去鏈結，選取剛剛建立的github repository
- 他就會抓取Dockerfile自動跑



## 建立一些成果分享

 
### Docker image for building OpenSSL for Android

其實會使用這些前提，如果你跟我一樣是:

- 在Windows 上寫Android App
- 極度討厭 Cygwin跟MinGW
- 對於build module一向講求，build完就不需要那些環境

之前做法當然笨笨的使用VM建了一個超肥的VM裡面也裝了一堆不知道有什麼東西．來build一些需要的module． opencv ffmpeg都是如此．

這次我改成自己Dockerfile 參考[這裡](https://github.com/kkdai/docker-android-openssl)

             
