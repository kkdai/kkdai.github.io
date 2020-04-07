---
layout: post
layout: post
author: kkdai
comments: true
date: 2006-07-04 11:21:07+00:00
slug: notes-to-migrate-from-vc6-to-vc2005
title: Notes to Migrate from VC6 to VC2005
wordpress_id: 429
categories:
- VC++程式設計
---

![](http://www.gotdotnet.com/team/ide/images/image002.jpg)

  1. **fstream.h 和iostream.h在VC8已經不存在 (參考[VinceYuan](http://vinceyuan.cnblogs.com/) Blog[升级VC7项目到VC8的注意事项](http://vinceyuan.cnblogs.com/archive/2005/12/20/301188.html))   
**#include <fstream>  
using namespace std;
  2. Depenency problem: 如果要引用某些其他project header file 在VC6中必須要設定dependency，在VC8中則沒有這樣的限制

其他詳細的步驟可以參考  
[**VinceYuan**](http://vinceyuan.cnblogs.com/)** Blog**[**升级VC7项目到VC8的注意事项**](http://vinceyuan.cnblogs.com/archive/2005/12/20/301188.html)
