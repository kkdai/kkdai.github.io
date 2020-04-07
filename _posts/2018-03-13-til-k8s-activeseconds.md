---
layout: post
title: "[Kubernetes] Use activeDeadlineSeconds to automatically terminate (force stop) your jobs"
description: ""
category: 
- Todayilearn
tags: ["k8s", "TIL"]
---



![](http://eslgamesworld.com/images/bomb.gif)

## 中文前言:

在使用 Kubernetes 的時候，可以選擇透過 Job 的方式來跑一次性的工作．但是如果希望你的工作在特定時間內一定得結束來釋放資源， 就得透過這個方式．

最近在研究這個的時候，發現有些使用上的小技巧，紀錄一下．

## Preface:

If you want to force to terminate your kubernetes jobs if it exceed specific time. (e.g.: run a job no longer than 2 mins). 

In this case you can use a watcher to monitor this Kubernetes jobs and terminate it if exceed specific time.  Or you can refer [K8S Doc:"Job Termination and Cleanup"](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/#job-termination-and-cleanup)  use `activeDeadlineSeconds` to force terminare your jobs.

## How to use `activeDeadlineSeconds`: 

It is very easy to setup `activeDeadlineSeconds` in spec.

```
apiVersion: batch/v1
kind: Job
metadata:
  name: myjob
spec:
  backoffLimit: 5 
  activeDeadlineSeconds: 100
  template:
    spec:
      containers:
      - name: myjob
        image: busybox
        command: ["sleep", "300"]
      restartPolicy: Never
```

In this example, this job will be terminated after 100 seconds (if it works well :p )

## Before you use activeDeadlineSeconds 

- If you ever run a job with `activeDeadlineSeconds`, you will need delete job before you run the same job again.
- The job will not stop if you run the same job name with `activeDeadlineSeconds`
- You will need change job name to make `activeDeadlineSeconds` works again. (suggest add specific tag in postfix of job name)

## Reference:

- [Kubernetes: issue Job pod does not terminate within activeDeadlineSeconds spec](https://github.com/openshift/origin/issues/10755)
- [Kubernetes document: Job Termination and Cleanup](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/#job-termination-and-cleanup)