---
description: >-
  Containers are not managed individually; instead, they are part of a larger
  object called a Pod.
---

# Pods

A Pod consists of one or more containers which share an IP address, access to storage and namespace. Typically, one container in a Pod runs an application, while other containers support the primary application (logging, proxy etc.).

## YAML Configuration

```
apiVersion: v1
kind: Pod
metadata: 
    name: mypod
spec:
    containers:
    - name: nginx
      image: nginx
...
```

## Editing Pods

Other than on deployments or other kubernetes resources, not all specifications of an existing pod can be edited. Pods can be edited in a few steps:

1. Extract YAML from current pod
   1. **`$ kubectl get pod webapp -o yaml > my-new-pod.yaml`**
2. Edit pod yaml&#x20;
   1. **`$ vim my-new-pod.yaml`**
3. Delete old pod
   1. **`$ kubectl delete pod webapp`**
4. Create new pod with edited yaml
   1. **`$ kubectl create -f my-new-pod.yaml`**

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create Pod</td><td><strong><code>$ kubectl create -f newpod.yaml</code></strong></td></tr><tr><td>Delete Pod</td><td><strong><code>$ kubectl delete -f newpod.yaml</code></strong></td></tr><tr><td>Update Pod</td><td><strong><code>$ kubectl apply -f newpod.yaml</code></strong></td></tr><tr><td>Show all Pods</td><td><strong><code>$ kubectl get pods</code></strong></td></tr><tr><td>Show details of Pods</td><td><strong><code>$ kubectl describe pod &#x3C;podname></code></strong></td></tr><tr><td>Show logs of container inside a Pod</td><td><strong><code>$ kubectl logs &#x3C;podname></code></strong></td></tr></tbody></table>
