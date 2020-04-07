---
layout: post
title: "[Kubernetes] GPU resource names in Kubernetes between Accelerators and DevicePlugin "
description: ""
category: 
- Todayilearn
tags: ["k8s", "TIL"]
---



#### ![](https://kerpanic.files.wordpress.com/2017/11/1qtnqtnrgq-n-r5k-nyuz0g.png)

## Two ways to enable GPU in Kubernetes:

If you want to enable GPU resource in Kubernetes and want Kubelet to allocate it.  You need config it as following ways:

- Kubernetes 1.7: Using NVidia container and  enable Kuberlet config `feature-gates=Accelerators=true`
- Kubernetes 1.9: Using [Device Plugin](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/) with Kuberlet specific config `feature-gates=DevicePlugins=true`

## Check node if it has GPU resource:

Using `kubectl` command `kubectl get node YOUR_NODE_NAME -o json`  to export all node info as json format. You should see something like:

```
## If you use Kubernetes Accelerator after 1.7
  "allocatable": {
        "cpu": "32",
        "memory": "263933300Ki",
        "alpha.kubernetes.io/nvidia-gpu": "4",
        "pods": "110"
    },
```

Detail defined in `k8s.io/api/core/v1/types.go`

    ## If you use Kubernetes Device Plugin after 1.9
      "allocatable": {
            "cpu": "32",
            "memory": "263933300Ki",
            "nvidia.com/gpu": "4",
            "pods": "110"
        },
## Reference:

- [Managing Compute Resources for Containers](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/)
- [Kubernetes: Device Plugin](https://kubernetes.io/docs/concepts/cluster-administration/device-plugins/)
- [NVIDIA-k8s-device-plugin](https://github.com/NVIDIA/k8s-device-plugin)