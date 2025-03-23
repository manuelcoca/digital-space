PersistenVolumeClaims objects are created for using the [PersistenVolume](persistentvolume.md) created by an administrator. Once the persistent volume claims are created, Kubernetes binds the persistent volumes to claims based on the request and properties set on the volume. During the binding process, Kubernetes tries to find a persistent volume that has sufficient capacity as requested by the claim, and any other request properties, such as access modes, volume modes, storage class, etc.

-   If Kubernetes should not bind automatically, labels and selectors can be used to bind to the right volume
-   If no volumes are available, the PVC will remain in a `pending` state

## YAML Configuration

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 4Gi
    selector: # Specifically select volumes to bind to by labels
        matchLabels:
            release: stable
```

### Using StorageClasses

If this PVC is created, the storage class associated with it, uses the defined provisioner, to provision a new disk with the required size on the cloud.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 8Gi
    storageClassName: azure-storage
```

### Using PVCs in Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: mypod
spec:
    containers:
        - name: myfrontend
          image: nginx
          volumeMounts:
              - mountPath: "/var/www/html"
                name: mypd
    volumes:
        - name: mypd
          persistentVolumeClaim:
              claimName: myclaim
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create PVC </td><td><strong><code>$ kubectl create -f ingress.yaml</code></strong></td></tr><tr><td>Show all PVCs</td><td><strong><code>$ kubectl get persistenvolumeclaim</code></strong></td></tr><tr><td>Delete PVC</td><td><strong><code>$ kubectl delete persistenvolumeclaim &#x3C;name></code></strong></td></tr></tbody></table>

> [!NOTE] persistenVolumeReclaimPolicy defined in the PV decides what should happen with the PV if a PVC is deleted
