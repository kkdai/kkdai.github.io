---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-04-27 04:31:09+00:00
slug: pythonmacopencv%e5%9c%a8mac%e4%b8%8a%e9%9d%a2%e5%88%a9%e7%94%a8python%e4%be%86%e6%93%8d%e4%bd%9copenbcv-using-opencv-in-mac-via-python
title: '[python][mac][OpenCV]在Mac上面利用Python來操作OpenbCV (Using OpenCV in Mac via python)'
wordpress_id: 1384
categories:
- opencv
- Python
---

最近不斷地為了[PyCon APAC 2014](https://www.google.com.tw/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CC4QFjAA&url=https%3A%2F%2Ftw.pycon.org%2F&ei=FdBbU-CPGoXokgWR94DIDQ&usg=AFQjCNE86SZ1r0Ha7LxhwVWLF67vEKcihA&sig2=leo7FMfmi0iZ7v-E0Dv4xg)在惡補Python，在臉書的討論區裡面看到這邊討論[OpenCV for Python的文章](https://www.facebook.com/groups/pythontw/permalink/10152836902068438/?stream_ref=2)．




有介紹到有人為了[OpenCV for Python 寫了專門的講解 blog](http://opencvpython.blogspot.tw/p/list-of-articles-in-this-blog.html)




好奇之下開始去尋找如何去使用[python在Windows](http://docs.opencv.org/trunk/doc/py_tutorials/py_setup/py_setup_in_windows/py_setup_in_windows.html)上面，想不到真是有點麻煩．




於是又把念頭動到Mac上面，想不到比想像中的簡單多了，參考這篇[(update-installing-opencv-on-mac-mountain-lion/)](http://www.jeffreythompson.org/blog/2013/08/22/update-installing-opencv-on-mac-mountain-lion/)






  * brew tap homebrew/science


  * brew install opencv


  * mkdir -p ~/Library/Python/2.7/lib/python/site-packages


  * echo '/usr/local/lib/python2.7/site-packages' > ~/Library/Python/2.7/lib/python/site-packages/homebrew.pth




找了一篇[測試的程式碼搭配著JPEG](https://github.com/abidrahmank/OpenCV2-Python/blob/master/Official_Tutorial_Python_Codes/1_introduction/modify_image.py)果然是成功的，於是開始去把自己之前[OpenCV Camera Testing Sample](https://gist.github.com/kkdai/9803015)拿來改成python版本．




要改其實不難，主要是要去一個一個把API從C++換成Python．可以去[官方API列表](http://docs.opencv.org/modules/highgui/doc/user_interface.html?highlight=cv.waitkey#cv.WaitKey)來找，幾乎都有兩個語言的版本．




要找詳細的code可以去看Github [https://github.com/kkdai/python_learning/blob/master/OpenCV_OpenCamera.py](https://github.com/kkdai/python_learning/blob/master/OpenCV_OpenCamera.py) 




目前開起相機是成功的，不過要抓取keycode有點問題還需要解決，解決完後就可以弄旋轉的部分．




 




雜話： 我真的不了解，身為一個軟體工程師有什麼理由不買Mac 的．就算你是Windows 的開發工程師，Mac可以直接裝Windows．更何況如果你是非Windows工程師，使用[Homebrew](http://brew.sh/)任何程式語言都可以馬上裝好．最近安裝不論是[Scala](http://www.scala-lang.org/)或是這次安裝OpenCV或是之前安裝Django就是那麼簡單．
