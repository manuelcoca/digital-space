# PersistentVolume

PersistentVolume object helps to manage storage more centrally. In a big cluster with many pods, it would be hard to manage the volumes in all Pods since a change would require to modify multiple pod definition files.

A PersistenVolume is a cluster wide pool of storage volumes managed by the administrator. Users then can select storage from the pool with [PersistenVolumeClaims](persistentvolumeclaim.md).

* Have a lifecycle independent of any individual pod
* You can define Reclaiming options to decide what should happen, if a Pod gets killed
  * Retain
  * Delete

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 5Gi                              # Amount of storage that should be reserved for this PersistenVolume
  volumeMode: Filesystem
  accessModes:                                # How a volume should be mounted on the hosts, whether in a read-only mode or read/write mode
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete       # Decides what should happen if a PersistenVolumeClaim is delete. In this case volume will be deleted as well.
  storageClassName: slow
  nfs:                                        # Volume type
    path: /tmp
    server: 172.17.0.2
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create PersistenVolume </td><td><strong><code>$ kubectl create -f ingress.yaml</code></strong></td></tr><tr><td>Show all PersistenVolumes</td><td><strong><code>$ kubectl get persistenvolume</code></strong></td></tr><tr><td>Show details of Ingress Resource</td><td><strong><code>$ kubectl describe ingress &#x3C;name></code></strong></td></tr></tbody></table>
