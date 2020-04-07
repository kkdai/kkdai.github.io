---
layout: post
title: "[DevOps] Mesos 與 DC/OS 安裝學習"
description: ""
category: 
- devops
tags: ["mesos", "vagrant", "docker", "DCOS"]

---

![](http://timothysc.github.io/images/mesos_logo.png)

## 安裝 Mesos

### 在本地端透過 Vagrant 來安裝 Mesos

可以參考 [Install Mesos via Vagrant](https://open.mesosphere.com/advanced-course/introduction/)．

### 心得:

雖然官方文件相當的清楚，但是 Vagrant 本身就有一些雷要踩．不論是

- [Vagrant SSH key insert failed.](http://stackoverflow.com/questions/22922891/vagrant-ssh-authentication-failure)
- [Vagrant Shared files between node](http://stackoverflow.com/questions/16704059/easiest-way-to-copy-a-single-file-from-host-to-vagrant-guest)
- [Vagrant node IP 亂跑的問題](http://stackoverflow.com/questions/22357624/why-isnt-vagrant-private-network-not-set-up-when-configured-within-provider-blo)

加上如果沒注意到記憶體跟 CPU 極有可能在 Mesos 裡面會出現無法 Scale Task 的狀況．

#### 關於雷的部分，講明白點....

1. `vagrant` 的 private IP 不是每次 `vagrant up` 都會正確，經常會跑掉．得要 `vagrant restart`．
2. `vagrant` 彼此間要共享檔案，可以透過 `/vagrant` 這個資料夾．不過 `/vagrant` 只是 guest OS 跟 host OS 溝通的共享資料夾，如果兩個 guest OS 要共享，還是得透過 host OS． 然而，共享並不是 real time 而是啟動的時候由 host 帶過去到 guest 的 `/vagrant`，如果要複製 guest file 到 host 是可以即時，如果要另外一個 guest 拿到，就得要重啟 guest．

## 安裝 Mesos CLI 流程

- 先安裝 `pip`
	- `curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py`
	- `sudo python get-pip.py`
- 在來安裝 virtualenv
	- `sudo pip install virtualenv`
- 安裝 `mesos.cli`
	- `sudo pip install mesos.cli`

## 在 GCE (Google Compute Engine) 安裝 DC/OS 

參考[這一篇](https://dcos.io/docs/1.7/administration/installing/cloud/gce/#install)

幾個東西要注意一下:

- 記得要先登入 Google Cloud 帳戶 `gcloud auth login`
- 在修改 [https://github.com/dcos-labs/dcos-gce](https://github.com/dcos-labs/dcos-gce) 安裝設定的 `group_vars/all` 的時候，記得以下資料:
	-  `bootstrap_public_ip`: 要參考原先設定 bootstrap 的 IP．必須確認該 IP 的格式與必須要確認一開始的 bootstrap 機器跟你要建立的 `master` 與 `agent` 是同個 subnet．
	-  `subnet`: 必須要設定一個新的 [subnet](https://cloud.google.com/compute/docs/subnetworks) (不能使用 `default`) 一定得建立一個新的 subnet ．
		-  不然會出現錯誤代碼如下: `The referenced subnetwork resource cannot be found`
-  修改 `hosts` 這個檔案裡面的資料:
	-  `master0 ip` 必須要給訂一個內部 IP ，這裡不能隨便填，也不能寫 `192.168.0.1` 這類的．必須符合 GCP 的內部 IP 格式．建議．
		-  不然會出現錯誤代碼如下 `Requested internal IP is outside the subnetwork CIDR range`

- 執行 `ansible-playbook -i hosts install.yml` 前必須確認自己有跑過 GCP 登入 `gcloud auth login` ．不然會一直卡在權限不足的錯誤． [ 20160803 更新: 不僅僅是單獨使用者需要登入 `gcloud` ，你必須要確認該機器的 root 也有登入過 `gcloud` ]
- 如果跑 `ansible-playbook -i hosts install.yml` 一直卡在 docker image 的問題，建議把 `sudo docker images` 清乾淨．
- 如果在最後的設定出現任何錯誤，記得確認 `master0` 是否有被建立，一直會出現錯誤．
	- 錯誤代碼如下: `The resource 'projects/YOUPROJECT/zones/asia-east1-c/instances/master0' already exists`


### Docker 1.12 更新:

20160803 由於 Docker 更新 1.12 後，目前無法順利安裝 DCOS ．

`Failed to start docker.service: Unit docker.socket failed to load: No such file or directory.`


### 安裝 DC/OS 成功後的登入

直接打開 `master0` 的 external IP 在網頁上就會出現 DC/OS 的登入畫面．

## 參考安裝教學

- [Mesos 官方安裝教學（使用 vagrant, docker)](https://open.mesosphere.com/advanced-course/installing-software/)
- [Mesos install using Google Cloud Platform](https://peihsinsu.gitbooks.io/docker-note-book/content/mesos-install.html)
- [AWS DC/OS Installation Guide (AWS)](https://dcos.io/docs/1.7/administration/installing/cloud/aws/)