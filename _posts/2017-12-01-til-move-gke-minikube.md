---
layout: post
title: "[TIL][Kubernetes] How to move your GKE into minikube"
description: ""
category: 
- TodayILearn
- Kubernetes
tags: ["TIL", "kubernetes", "minikube", "docker"]

---

![](https://cdn-images-1.medium.com/max/1200/1*fMjeHmaGDI5UIzzvyDuUoQ.png)

## Preface

As a cloud company, we usually build our container cluster on cloud platforms such as GCP or AWS. but sometimes we need to think about how to take to go solution to our customer. So, here comes a challenge - Scale-in our cloud platform and put into pocket (mmm I mean "[minikube](https://github.com/kubernetes/minikube)")

## An example GKE service

Here is a simple example which you (might) build in GKE.

- `DB`: MongoDB with stateful set binding with google persistent disk. Use stateful set from ("[Running MongoDB on Kubernetes with StatefulSets](http://blog.kubernetes.io/2017/01/running-mongodb-on-kubernetes-with-statefulsets.html)") as an example 
- `Web service(fooBar)`: a golang application which accesses mongo DB with the load balancer.  Because `fooBar` is proprietary application which `fooBar` image store in GCR (Google Cloud Registry) not in docker hub.

## How to migrate your service from GKE to minikube:

We will just list some note to let you know any tip or note you migrate your service to minikube.

### 1. Migrate Persistent Volume to minikube.

"[Here](http://blog.kubernetes.io/2017/01/running-mongodb-on-kubernetes-with-statefulsets.html)" is a good article to understand StatefulSet. But however it use some resource you could not use in minikube, such as:

- [Kubernetes Persistent Volumes for GCP persistent disk](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

Let me put all yaml setting and give more detail.

```
apiVersion: apps/v1beta1
kind: StatefulSet
metadata:
  name: mongo
spec:
  serviceName: "mongo"
  replicas: 3
  template:
    metadata:
      labels:
        role: mongo
        environment: test
    spec:
      terminationGracePeriodSeconds: 10
      containers:
        - name: mongo
          image: mongo
          command:
            - mongod
            - "--replSet"
            - rs0
            - "--smallfiles"
            - "--noprealloc"
          ports:
            - containerPort: 27017
          volumeMounts:
            - name: mongo-persistent-storage
              mountPath: /data/db
        - name: mongo-sidecar
          image: cvallance/mongo-k8s-sidecar
          env:
            - name: MONGO_SIDECAR_POD_LABELS
              value: "role=mongo,environment=test"
  volumeClaimTemplates:
  - metadata:
      name: mongo-persistent-storage
      annotations:
        volume.beta.kubernetes.io/storage-class: "fast"
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 100Gi
```


The `volumeClaimTemplates` provide stable storage using PersistentVolumes provisioned by a PersistentVolume Provisioner. (Use "Google Persistent Disk" on GCP)

The "fast" will specific to use google persistent disk with "SSD" performance. So, in this case our Kubernetes volume claim will specific to **google cloud persistent disk**.

#### How we migrate to Google Persistent Disk volume into minikube?

There are two solutions for this:

- Use `hostPath` directly.
- Still using volume claim but **remove google persistent disk annotation**.

In the second case, we could still use `volumeClaimTemplates` and just remove volume provider persistent disk `annotation`. It will select the best for our system. (for now, it is `hostPath`.

```
  volumeClaimTemplates:
    - metadata:
        name: minikube-clain
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 10Gi
```

### 2. Using GCR in minikube

When you deploy GKE, you usually use GCR (Google Cloud Registry) to storage your docker container. If you want to move your project from GKE to minikube, you have solutions as follows:

1. Build your own docker registry. But you will need handle follow things:
    - Use `--insecure-registry` if you use the self-signed key.
    - Use public DNS to request CA key (It will against we want to move into minikube)

2. Use minikube to connect to GCR directly.
    - Minikube has add-on `minikube addons configure registry-creds`
    - Use temproary token solution (around ~30 mins), [here](https://ryaneschinger.com/blog/using-google-container-registry-gcr-with-minikube/) is the reference.

```
kubectl delete secret gcr

kubectl create secret docker-registry gcr \
    --docker-server=https://asia.gcr.io \
    --docker-username=oauth2accesstoken \
    --docker-password="$(gcloud auth print-access-token)" \
    --docker-email=yout_login_gmail@gmail.com
```



[Here](https://github.com/kubernetes/minikube/blob/master/docs/insecure_registry.md#private-container-registries) is the document from minikube.

## Launch your service

Ok, that's done. If you are wondering how to connect to your web service `fooBar`. You can just call `minikube service fooBar`.


# Troubleshooting

### using busybox to debug

It is very useful when you don't know what's happening on your pod, especially for networks storage or network.

```
cat <<EOF | kubectl create -f -
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: busybox
spec:
  selector:
    matchLabels:
      app: busybox
  replicas: 1
  template:
    metadata:
      labels:
        app: busybox
    spec:
      containers:
      - name: busybox
        image: busybox
        command:
          - sleep
          - "86400"
EOF

kubectl exec -it <BUSYBOX_POD_NAME> -- nslookup redis.default
```

**example:**

```
kubectl exec -it busybox-7fc4f6df6-fhxnp -- nslookup redis.default
```

### Destroy & Cleanup the minikube

Remember to remove the setting file when you uninstall minikube.

```
minikube delete
rm -rf ~/.minikube
```

### Cannot start minikube

It might happen as follow reasons:

1. minikube version change.
2. docker version change.
3. VM has already been destroyed by VirtualBox.


```
Starting local Kubernetes v1.8.0 cluster...
Starting VM...
E1124 22:16:44.080926   87887 start.go:150] Error starting host: Temporary Error: Error configuring auth on host: Temporary Error: ssh command error:
```

Solution: `rm -rf ~/.minikube`