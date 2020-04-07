---
layout: post
title: "[DevOps] 學習 Kubernetes 筆記之一: 透過單機版的 Kubernetes (miniKube) 來玩 K8S "
description: ""
category: 
- devops
tags: ["kubernetes", "docker"]

---

![](http://blog.arungupta.me/wp-content/uploads/2015/01/kubernetes-logo.png)

##  miniKube 單機版的 Kubernetes

[miniKube](https://github.com/kubernetes/minikube) 是 Google 發布可以在單機上面跑 Kubernetes 的工具，安裝跟使用都相當簡單．由於會在本地跑一個 VM ，所以也不用擔心會被 Google Cloud 不小心付費的問題． 

### 安裝

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.6.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

### 啟動

```
minikube start
```

### 連接進該 VM

```
minikube ssh
```

### 打開並且顯示 minikube 的 dashboard

```
minikube dashboard
```



## 接下來，安裝 Kubernetes

### 安裝 Google Cloud SDK

```
curl https://sdk.cloud.google.com | bash
```

安裝好之後，會有 `gcloud`, `gsutil` 但是還需要安裝 `kubectl`

### 透過 Google Cloud SDK 安裝 Kubernetes

```
gcloud components install kubectl
```

### 簡單 Tutorial

```
# Startup miniKube
minikube start

# Create a deployment
kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080

# Expose it
kubectl expose deployment hello-minikube --type=NodePort

# Check pod
kubectl get pod

# Scale 
kubectl scale deployment hello-minikube --replicas=4

# Direct link to hello-minikube 
curl $(minikube service hello-minikube --url)

```

### How to Rolling Update on Kubernetes

```
kubectl edit deployment hello-minikube
```

Update your service version and apply it (save it back).

You can also update this file via `minikube dashboard`

- Edit "deployment"
- Update "spec"->"spec"-> "image" version.


## Kubernetes 與 Docker Swarm Mode 差異

- Kubernetes 的 Pods 就是 Docker Swarm 的 Service
- 對於 *Rolling Update*:
	- **Kubernetes** : 先起來新的 Pod 是新的版本，然後關閉舊的 Pod． 也就是原先有 3 個 Pod ，為了要 Rolling Update 於是產生另外 3 個新的 Pod ．等到穩定後，把舊的 3 個 Pod 關閉．
	- 	**Docker Swarm**: 透過 Load Balancer 把沒有用的先換成新的．
-  對於 *Routing Mesh* :
	-  **Kubernetes** : 並沒有這個功能，因為對外需要透過 Expose
	-  **Docker Swarm** : 每個 Node 都具有 Routing Mesh ，也就是都可以連接到 Service


## 參考安裝教學

- [Kuberbetes學習筆記](https://www.gitbook.com/book/gcpug-tw/kuberbetes-in-action/details)
- [Kubernetes: Hello World Walkthrough](http://kubernetes.io/docs/hellonode/)
- [Github: miniKube ](https://github.com/kubernetes/minikube)
- [Github: Kubernetes](https://github.com/kubernetes/kubernetes)