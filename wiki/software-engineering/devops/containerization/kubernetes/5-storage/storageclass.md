If a PersistenVolume would use a Cloud Storage volume, the volume on the cloud must be created and provisioned before creating a PersistenVolume. This requires the admin to manually provision cloud volumes which is not scaleable.

StorageClass helps with dynamic provisioning of volumes in the Cloud, which means a volume gets provisioned when an application requires it.

By creating a StorageClass there is no need to create PersistentVolumes anymore, because they will be created automatically by the StorageClass object.

StorageClass are used by the PersistenVolumeClaims -> [see here ](persistentvolumeclaim.md#using-storageclasses)

## YAML Configuration

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: google-storage
provisioner: kubernetes.io/gce-pd
```
