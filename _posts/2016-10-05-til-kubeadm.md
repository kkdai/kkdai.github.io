---
layout: post
title: "[Kubernetes] 透過 Kubernetes 1.4 新功能 Kubeadm 來建立 Kubernetes Cluster"
description: ""
category: 
- Golang
tags: ["kubernetes", "docker"]

---

![](http://blog.arungupta.me/wp-content/uploads/2015/01/kubernetes-logo.png)

## 緣起

2016/09/26 Google 推出了新版的 [Kubernetes 1.4](http://blog.kubernetes.io/2016/09/kubernetes-1.4-making-it-easy-to-run-on-kuberentes-anywhere.html?m=1) 裡頭最讓人期待的，就是 `kubeadm` 這個新的功能．

本來很期待他能夠讓 Kubernetes 變得更容易安裝並且加入新的節點，所以就試玩了一下．發現功能都還蠻強大的．不僅僅是變得更容易安裝，其實根本就是跟 `docker swarm mode` 致敬啊．整個變得簡單到不行．

## 建立機器

這邊只建議兩個 OS 版本:

- Ubuntu 16.04
- CentOS 7

`備註`: 如果你使用 `docker-machine` 來建立 VM 獲釋 GCP 上面的機器，預設的 Ubuntu 是 15.10 ，是無法順利安裝完畢的．

## 下載相關的 binary

每一台電腦（包括 Master 跟 Node 都得執行）所有的指令都是透過 `sudo su -`

```
# curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
# cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
# apt-get update
# apt-get install -y docker.io kubelet kubeadm kubectl kubernetes-cni
```

## 啟動 Master

就像是 `docker swarm mode` 的 master 一樣，只要一行指令就好．

```
kubeadm init
```

詳細會出現的螢幕如下

```
# kubeadm init
<master/tokens> generated token: "306d01.da88f33b20103873"
<master/pki> created keys and certificates in "/etc/kubernetes/pki"
<util/kubeconfig> created "/etc/kubernetes/kubelet.conf"
<util/kubeconfig> created "/etc/kubernetes/admin.conf"
<master/apiclient> created API client configuration
<master/apiclient> created API client, waiting for the control plane to become ready
<master/apiclient> all control plane components are healthy after 51.538064 seconds
<master/apiclient> waiting for at least one node to register and become ready
<master/apiclient> first node is ready after 0.504944 seconds
<master/discovery> created essential addon: kube-discovery, waiting for it to become ready
<master/discovery> kube-discovery is ready after 13.506671 seconds
<master/addons> created essential addon: kube-proxy
<master/addons> created essential addon: kube-dns
Kubernetes master initialised successfully!
You can now join any number of machines by running the following on each node:
kubeadm join --token 306d01.da88f33b20103873 10.240.0.11
```

`備註`: 當出現 `waiting for at least one node to register and become ready` 需要大概幾十秒鐘（取決你機器的速度）．

其他機器只要透過 `kubeadm join --token 306d01.da88f33b20103873 10.240.0.11` 就可以了．

## Node 加入該群集 (Clustering)

```
kubeadm join --token 306d01.da88f33b20103873 10.240.0.11
```

 詳細指令顯示如下：
 
```
# kubeadm join --token 306d01.da88f33b20103873 10.240.0.11
<util/tokens> validating provided token
<node/discovery> created cluster info discovery client, requesting info from "http://10.240.0.11:9898/cluster-info/v1/?token-id=306d01"
<node/discovery> cluster info object received, verifying signature using given token
<node/discovery> cluster info signature and contents are valid, will use API endpoints [https://10.240.0.11:443]
<node/csr> created API client to obtain unique certificate for this node, generating keys and certificate signing request
<node/csr> received signed certificate from the API server, generating kubelet configuration
<util/kubeconfig> created "/etc/kubernetes/kubelet.conf"
Node join complete:
* Certificate signing request sent to master and response
  received.
* Kubelet informed of new secure connection details.
Run 'kubectl get nodes' on the master to see this machine join.
```

## 驗證 Kubernetes 叢集有成功建立

```
# kubectl get nodes
NAME               STATUS    AGE
evan-kube-1        Ready     1m
evan-kube-2        Ready     1m
evan-kube-master   Ready     4m
```

## 建立 Clustering 的 network

```
# kubectl apply -f https://git.io/weave-kube
daemonset "weave-net" created
```

## 跑一些簡單的範例吧

透過 Console 直接建立兩個 Pods ，以下指令需要在 Master 輸入．

```
# kubectl get nodes
NAME               STATUS    AGE
evan-kube-1        Ready     1m
evan-kube-2        Ready     1m
evan-kube-master   Ready     4m
root@evan-kube-master:~# cat <<EOF | kubectl create -f -
> apiVersion: v1
> kind: Pod
> metadata:
>   name: busybox-sleep
> spec:
>   containers:
>   - name: busybox
>     image: busybox
>     args:
>     - sleep
>     - "1000000"
> ---
> apiVersion: v1
> kind: Pod
> metadata:
>   name: busybox-sleep-less
> spec:
>   containers:
>   - name: busybox
>     image: busybox
>     args:
>     - sleep
>     - "1000"
> EOF
pod "busybox-sleep" created
pod "busybox-sleep-less" created
```


確認一下 Pod 有建立成功

```
# kubectl get pods
NAME                 READY     STATUS    RESTARTS   AGE
busybox-sleep        1/1       Running   0          6s
busybox-sleep-less   1/1       Running   0          6s
```

確認一下每個 Pod 在哪個節點執行 ?

```
# kubectl describe pod busybox-sleep
Name:           busybox-sleep
Namespace:      default
Node:           evan-kube-2/10.240.0.14
...
```

這樣就可以知道 pod `busybox-sleep` 在 `evan-kube-2` 這台機器．

再來確認另外一個 pod `busybox-sleep-less` 在哪個節點執行 ?

```
# kubectl describe pod busybox-sleep-less
Name:           busybox-sleep-less
Namespace:      default
Node:           evan-kube-1/10.240.0.13
```

這樣就可以知道 pod `busybox-sleep-less` 在 `evan-kube-1` 這台機器．

## 限制 (Limitations)


- 目前 `kubeadm` 還不支援 cloud-provider integrations, 所以如果需要 [Load Balancers (LBs)](http://kubernetes.io/docs/user-guide/load-balancer/) 或是 [Persistent Volumes (PVs)](http://kubernetes.io/docs/user-guide/persistent-volumes/walkthrough/) ．還是建議使用 GKE 
- 這樣建立的叢集只有一個 Master ，並且上面的 etcd 只有一個，如果需要備援或是要做 HA 就建議透過 [etcd clustering](https://coreos.com/etcd/docs/latest/admin_guide.html) 來做．
- 目前 `kubectl logs` 還不支援 `kubeadm` 建立出的叢集，請看 issue [22770](https://github.com/kubernetes/kubernetes/issues/22770).


## Other Links

- [Kubernetes 1.4: Making it easy to run on Kubernetes anywhere](http://blog.kubernetes.io/2016/09/kubernetes-1.4-making-it-easy-to-run-on-kuberentes-anywhere.html?m=1)
- [Installing Kubernetes on Linux with kubeadm](http://kubernetes.io/docs/getting-started-guides/kubeadm/)
- [Creating Single-Container Pods](http://kubernetes.io/docs/user-guide/pods/single-container/)
- [kubectl-cheatsheet](http://kubernetes.io/docs/user-guide/kubectl-cheatsheet/)