# Volumes

Since Containers in Docker and Pods in Kubernetes are meant to be transient which mean they are meant to last only for short period of time it is required to store data beyond a containers/pods lifecycle.

* Volumes needs a storage
* Volumes are shared by every pod
  * If you create a deployment with 5 replicas, all pods will share the same volume
  * Sometimes you don't want this -> [PersistentVolume](persistentvolume.md)

## Volume Types

Kubernetes supports several types of volumes:

### emptyDir

An `emptyDir` volume is first created when a Pod is assigned to a node, and exists as long as that Pod is running on that node (stored in memory). When a Pod is removed from a node for any reason, the data in the `emptyDir` is deleted permanently

### hostPath

A `hostPath` volume mounts a file or directory from the host node's filesystem into your Pod (data stored on the nodes).&#x20;

{% hint style="info" %}
HostPath volumes present many security risks, and it is a best practice to avoid the use of HostPaths when possible, especially in production. When a HostPath volume must be used, it should be scoped to only the required file or directory, and mounted as ReadOnly.
{% endhint %}

Example use-cases for a `hostPath` are:

* running a container that needs access to Docker internals; use a `hostPath` of `/var/lib/docker`
* running cAdvisor in a container; use a `hostPath` of `/sys`
* allowing a Pod to specify whether a given `hostPath` should exist prior to the Pod running, whether it should be created, and what it should exist as

Also not recommended on multi-node cluster because pods would always use the path configure unter `hostPath` to access the data. But the data will not be in sync accross multiple nodes&#x20;

```
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /opt
      name: data-volume
  volumes:
  - name: data-volume
    hostPath:
      # directory location on host
      path: /data
      # this field is optional
      type: Directory
```

<div align="left">

<figure><img src="../../.gitbook/assets/IMG_6159 2.JPG" alt="" width="375"><figcaption></figcaption></figure>

</div>

### nfs

An `nfs` volume allows an existing NFS (Network File System) share to be mounted into a Pod

[https://kubernetes.io/docs/concepts/storage/volumes/#nfs](https://kubernetes.io/docs/concepts/storage/volumes/#nfs)

```
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: registry.k8s.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /my-nfs-data
      name: test-volume
  volumes:
  - name: test-volume
    nfs:
      server: my-nfs-server.example.com
      path: /my-nfs-volume
      readOnly: true
```
