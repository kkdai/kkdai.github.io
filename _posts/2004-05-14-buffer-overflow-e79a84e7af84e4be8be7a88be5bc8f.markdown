---
layout: post
layout: post
author: kkdai
comments: true
date: 2004-05-14 10:51:31+00:00
slug: buffer-overflow-%e7%9a%84%e7%af%84%e4%be%8b%e7%a8%8b%e5%bc%8f
title: Buffer Overflow 的範例程式
wordpress_id: 90
categories:
- STL學習心得
---

關於buffer overflow的範例程式，就如同[上一篇文章](http://www.evanlin.com/blog/archives/000112.htm)裡面有提到，buffer
overflow
主要是利用C++對於陣列大小沒有限制的概念，當你輸入一個過大的數值，回傳值(return
value)會被蓋掉，即使你輸入錯誤的數值，你會也因為這樣而成功的登入電腦（或是使某個安全認證通過～～～）


<!-- more -->


一個好朋友[(生魚片)寫了一個範例程式](http://maxeii.adsldns.org/archives/000044.html)，但是我看了一下總是覺得哪裡怪怪的，因為我記得雖然記憶return
value會在記憶體位置之中，但是很難確切抓出function的回傳位置就在變數之後，並且他設定的函數為void，照理說也是無法回傳的才對～所以我大膽猜測他所寫的程式主要是將func的副程式加以執行過後的結果，與我們原先探討的Buffer
Overflow有所差異，所以我寫了一個範例程式如下：




#include 
#include 

char func(void)
{



  char *p;
  char r_c;
  char buf[1];

  p=buf;
  *p='N';
  *(p+1)='N';
  *(p+2)='N';
  *(p+3)='N';
  *(p+4)='N';
   if (buf[0]=='y'){
	  r_c='N';}
	  return r_c;


}



int main(int argc, char* argv[])
{
	char Return_char;
		if 	 (func()=='N'){
			printf("system ~~~ passing~~~n");
		}
		else{

			printf("system is not passing~~~n");
		}


}




在這個範例程式之中，副函示有數個變數宣告，buf[1],*p,
r_c 所以記憶體分配方式如下_(先宣告的記憶體位址比較後面，後宣告的記憶體位置比較前面）_：



    
                   |
    



    
                                   +-----------------------+
                                   | (other variable)      |
                                   +-----------------------+
                                   | 指標 p                |
                                   +-----------------------+
                                   | (other variable)      |
                                   +-----------------------+
                                   | r_c (1 bytes)         |
                                   +-----------------------+
                                   | (other variable)      |
                                   +-----------------------+
                                   | buf (1 bytes)         |
                                   +-----------------------+
                                   | (other data in stack) |
                                   |           .           |
                                   +-----------------------+
                                   | return address of     |
                                   | this function         |
                                   |           .           |
                                   |           .           |
    





當你在VC++編輯的時候，若是使用 [F11]來一個階段，一個階段的DEBUG，去一個個參照變數的位置(Address)的時候，你會發現以下的結果


[
![按下去看全圖](http://www.evanlin.com/blog/archives/0514/0514-1.jpg)

](http://www.evanlin.com/blog/archives/0514/0514-1.jpg)


也就是透過指標*p，回傳變數r_c會被該更改到，使得回傳的值變的有誤。




所以這個程式明明不該輸出『System passing
』卻因為回傳值變成r_c所以產生～～～～錯誤造成程式的錯誤




　
