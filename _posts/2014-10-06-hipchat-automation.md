---
layout: post
layout: post
title: "[學習心得]來玩玩Hipchat.. 第一篇"
description: ""
category: 
- 學習文件
- Hipchat
tags: []

---

![image](https://www.hipchat.com/img/design_align/hipchat-logo.svg)

主要是在管的一些伺服器會有莫名其妙的service中斷的狀況，本來有在增加一些log．不論是在MySQL還是程式本身．但是除了要找出原因之外，要能夠迅速的回復服務也是一個方式． 

其中想過不少方法，不過也都有撞牆的狀況：

- 使用SMTP，寄送Email (用Gmail account)
    - 發現Google的安全機制太麻煩了，不僅僅會擋掉其他地區的登入．還會要求你使用網頁版的先登入過gmail 才能使用程式去跑SMTP．
    - 這裡可以參考一些好用的Python Send Email:
        - [http://stackoverflow.com/questions/10147455/trying-to-send-email-gmail-as-mail-provider-using-python](http://stackoverflow.com/questions/10147455/trying-to-send-email-gmail-as-mail-provider-using-python)
- 使用XMPP (或是使用Facebook Chat)        
    - 其實Facebook 的聊天有支援XMPP的，使用Pidgin也可以順利登入．但是考量到Aliyun在大陸內部臉書被擋．放棄！
    - 參考一下[Facebook Chat](https://www.facebook.com/sitetour/chat.php)
- 還是回過頭來使用最被大家所推薦的[Hipchat](https://www.hipchat.com/)，不僅僅支援的服務多．也可以自己再去手動修改．
    - 裡面比較需要注意的是 API Token 有分 1.0 跟 2.0，大部分外面的應用都是使用API 1.0
    - 這邊有一個比較好用的 CLI的方式，可以直接傳送訊息 [Hipchat-cli](https://github.com/hipchat/hipchat-cli)
    
    
這才剛開始.. 希望能把自動化弄得更容易...     

**參考link**

- Hubot 使用node.js 架設在hipchat的機器人系統
    - [https://hubot.github.com/](https://hubot.github.com/) 
- 關於hipchat xdite的看法
    - [http://blog.xdite.net/posts/2013/04/02/move-to-hipchat](http://blog.xdite.net/posts/2013/04/02/move-to-hipchat) 
- StreetVoice如何使用 HipChat打造自動化機器人
    - [https://speakerdeck.com/tzangms/the-workflow-of-the-new-streetvoice](https://speakerdeck.com/tzangms/the-workflow-of-the-new-streetvoice)
- 把Hipchat當成log 小鋪    
    - [http://demo.tc/Post/799](http://demo.tc/Post/799)         
    
