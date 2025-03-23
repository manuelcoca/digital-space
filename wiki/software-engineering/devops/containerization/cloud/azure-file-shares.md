By default, Azure Container Instances are stateless. For persistence, mount an Azure file share created with Azure Files.

Azure Files offers fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) protocol

## Limitations

-   You can only mount Azure Files shares to Linux containers.
-   Azure file share volume mount requires the Linux container run as _root_.
-   Azure File share volume mounts are limited to CIFS support.

## Mount File Shares

### Via CLI

```shell
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image mcr.microsoft.com/azuredocs/aci-hellofiles \
    --dns-name-label aci-demo \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

### Via YAML

```yaml
apiVersion: "2019-12-01"
location: eastus
name: file-share-demo
properties:
    containers:
        - name: hellofiles
          properties:
              environmentVariables: []
              image: mcr.microsoft.com/azuredocs/aci-hellofiles
              ports:
                  - port: 80
              resources:
                  requests:
                      cpu: 1.0
                      memoryInGB: 1.5
              volumeMounts:
                  - mountPath: /aci/logs/
                    name: filesharevolume
    osType: Linux
    restartPolicy: Always
    ipAddress:
        type: Public
        ports:
            - port: 80
        dnsNameLabel: aci-demo
    volumes:
        - name: filesharevolume
          azureFile:
              sharename: acishare
              storageAccountName: <Storage account name>
              storageAccountKey: <Storage account key>
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```
