---
layout: post
layout: post
title: "[Git]使用git subtree與git submodule來拆解與管理專案"
description: ""
category: 
- Git
tags: []

---

![image](http://git-scm.com/images/logo@2x.png)


### 前言:

我想這樣的需求應該也不少，原本在專案的開發過程中，可能在一個respository會變得相當的大而且相當的肥．這時候可能要把一些專案拆解出去． 但是又希望有一個overview的專案在外面可以查看，研究了一下利用[sourcetree](http://www.sourcetreeapp.com/)想要達到這樣的方式，但是看起來還是得用git cli來使用．



### 範例: 

以下的解釋都會透過這個範例，假設我們現在一有一個project_sample裡面有feature_a與feature_b．但是feature_a由於太受歡迎(真有這種feature???)所以很多產品都要使用，必須要獨立出來開發．

**資料結構如下:**

- project_sample
    - feature_a
    - feature_b

這時候，我們要把feature_a獨立出來成為另外一個repository並且希望開發project_sample的人可以一樣看得到feature_a．


### SourceTree沒有你想像的好用

![image](http://www.sourcetreeapp.com/images/logoSourceTree.png)

[Sourcetree](http://www.sourcetreeapp.com/)是一套提供GUI的git tool，我個人不論是在Mac/Windows都有使用這套軟體．雖然bug不少，倒也算是涵蓋了我常用的幾個功能．不過要使用接下來的 [git subtree split](https://help.github.com/articles/splitting-a-subfolder-out-into-a-new-repository/) 就無法使用 sourcetree的UI，得用command line．


### 詳細步驟:切割repo

接下來的詳細步驟可以參考這個[StackOverflow](http://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository/17864475#17864475)，以下以我提出的範例來做示範 :

- 在project_foo 下面，使用command line

    //這裏要注意的是: 如果是在Windows 路徑要使用 A/B/C 而不是 A\B\C
    
        git subtree split -P feature_a -b "branchA"

- 建立出新的branch之後，你會發現所有change list被切成兩個branch．一個是原本project_sample全部，另一個只有feature_a
- 接下來要把branchA submit到新的repository，建議路徑先不要在相同目錄下


        mkdir <new-repo>
        cd <new-repo>
        git init
        git pull ../projhect_sample branchA
        git remote add origin <git@some-new-repro>
        git push origin -u master
    
- 如此一來會把剛剛另外一個branchA的所有內容submit到新的repo去．回來到就的repo，如果就的repo不需要留著原來的部分(我們接下來會用submodule import)，就必須依照以下方式:

    // 如此會將所有與feature_a 目錄有關的檔案移除
    
        git rm -rf feature_a


### 關於Subtree與Submodule取捨

如此一來，光是切割的部分已經算是完成了． 這個部分無法使用[Sourcetree](http://www.sourcetreeapp.com/)來完成．  接下來要把原來的Project_Sample來把feature_a作為一個submodule來輸入．

這裏我曾經有試過透過git subtree add的方式，把feature_a加回原來的project_sample．不過這樣sumit之後會把所有的code都submit而sub repo "feature_a"修改後無法正確的對應到 project_sample． (只有第一個人可以，其他clone下來的人會看成一個完整的repo 而喪失了subtree的link)

或許這個部分可能是操作上的錯誤． 持續研究中，我使用的是 submodule 來管理project_sample中的 feature_a．


### 詳細步驟:管理sub repo

接下來，可以使用[Sourcetree](http://www.sourcetreeapp.com/)來管理feature_a．

    [Repository]->[Add Submodule..] 

選擇好你的feature_a，直接就可以Add Submodule進來． 由於是透過[Sourcetree](http://www.sourcetreeapp.com/)所以會直接clone下來並且在左邊會出現一個Submodules

**更新Submodule:**

接下來，如果你要更新Submodule裡面的程式碼，需要把滑鼠移到Submodules，並且選擇[Open Submodule]

    [Fetch] -> [Pull] //In Submodule

如此一來，你僅僅是你local把submodule的部份更新了．如果要所有的人都更新最新版本的submodule 還是需要submit submodule的tag．

    [Commit]->[Push] // submodule, in project_sample

**修改Submodule:**

如果要修改，一樣選擇[Open Submodule]

    [Commit]->[Push]  //In Submodule

除了，local Push之外．你還需要sync submodule的tag． 回到原來的 project_sample commit "submodule"．

    [Commit]->[Push] // submodule, in project_sample


###參考文章:

- [Stackoverflow: Detach subdirectory into separate Git repository](http://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository/17864475#17864475)
- [GitSCM: 6.6 Git 工具 - 子模組 (Submodules)](http://git-scm.com/book/zh-tw/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E7%B5%84-Submodules)
- [第十三號艦隊: git subtree](http://hungmingwu-blog.logdown.com/posts/178731-git-subtree)
- [WMの物語: 使用 git subtree 來分拆子目錄成獨立的新 repo](http://blog.kidwm.net/341)
