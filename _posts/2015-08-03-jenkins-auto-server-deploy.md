---
layout: post
layout: post
title: "[JENKINS]使用Jenkins部署Golang Server"
description: ""
category: 
- golang
- ci_continuous integration
tags: []

---


##前言

想要把Server部署(deployment)自動化，於是又把Jenkins拿出來用．


### Service Deployment

#### PHP Service

要部署PHP Service其實很簡單，主要就是把檔案*.php複製到apache目錄底下就好．

**在Jenkins抓完Git之後，新增一個"執行Shell"**

        echo "Deploy server1..."
        mv pserver1/*.php /var/www/html/server1/

        

#### Go Service 

由於Server上的Golang Service使用[screen](http://www.arthurtoday.com/2013/12/ubuntu-screen-quickly-switch-between-terminals.html)在背景跑．要部署要分成幾個階段:

- Build Go execution file
    - 這裏有兩個approach一個是使用[Go Plugin for Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Go+Plugin)，而我是直接使用command line跟系統參數．確認`go env`可以抓到正確的環境參數:

**在Jenkins抓完Git之後，新增一個"執行Shell"**

        echo "Building Go SERVER"
        export GOPATH=/YOUR/GOPATH
        cd YOUR_GO_PATH
        rm ORI_GO_EXECUTION_FILE
        go build -v

- Kill current service and start new service

**在Jenkins build好Go之後，新增一個"執行Shell"**

        echo "Deploy Server"
        cd /YOUR/PATH
        sudo ./go_server_restart.sh


這裏有shell script 的內容:

        #!/bin/sh
        cd /YOUR/PATH
        COMMAND_GO_SERVER=/YOUR/PATH/go_server
        go_server_pid=`pidof ${COMMAND_GO_SERVER}`
        if [  $go_server_pid ]; then
                date +"%Y/%m/%d %H:%M:%S-Go Server Restart"
                kill -9 $go_server_pid
                sudo screen -d -m ./go_server
                date +"%Y/%m/%d %H:%M:%S-Restart completed"
        else
                sudo screen -d -m ./go_server
                date +"%Y/%m/%d %H:%M:%S-Start completed"
        fi    


### 關於Jenkins權限


這裏要注意，如果要跑`mv` 或是`kill -9` 需要有root的權限，這樣需要把jenkins的權限調高．細節可以參考[How to run a script as root in Jenkins?](http://stackoverflow.com/questions/11880070/how-to-run-a-script-as-root-in-jenkins)

- 修改Sudoers
    - `sudo visudo`
    - 增加`jenkins    ALL = NOPASSWD: ALL`

### 如果要更改檔案後commit回去 [2015/10/21 update]

如果希望能更改檔案後，自動的push回去的話． 主要要參考這一個[stackoverflow](http://stackoverflow.com/questions/19922435/how-to-push-changes-to-github-after-jenkins-build-completes/19923066#19923066)

主要的問題是jenkins的每一個action裡面的git 是沒有紀錄的（因為是工作區) 所以解決方式是跑`git checkout master`．不過這樣一來在那邊的git 的id 會亂掉． 比較建議的做法是另外開一個資料夾，然後把檔案複製過去再去commit

- jenkins a repo build file
- copy destination files to b folder (the same a repo)
- git commit/push in b folder


**請注意: 這個做法有相對的安全考量，請自行參照**

### TODO:

#### Post commit hook

需要Jenkins在commit submit之後，自動跑task． 可以參考[Jenkins CI: How to trigger builds on SVN commit](http://stackoverflow.com/questions/10014252/jenkins-ci-how-to-trigger-builds-on-svn-commit)


##參考資料

- [How to run a script as root in Jenkins?](http://stackoverflow.com/questions/11880070/how-to-run-a-script-as-root-in-jenkins)
- [How To Edit the Sudoers File on Ubuntu and CentOS](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)
