StatefulSets are the same as Deployments but allows additional logic to scale up and down pods in order. Deployments scale up Pods all at one which can be a problem on some use-cases. Example:

-   Running 3 database Replicas and using the Master-Slave topology. In this example, the master database must be created first, then the Slave-01 which gets a clone of data from master and then Slave-02 which gets a clone of data from Slave-01. Here the order of scaling the Pods is important
-   Another issue of Deployments in this example is, that the Pods can't be designated as master and other as slave. IP-Address or Pod names are not reliable especially if a Pod is recreated

With StatefulSets the above problems can be solved:

-   Each Pod gets and index in order to scale up and down in order
    -   Before the next Pod is created, all of its predecessors must be `Running` and `Ready`
    -   Before a Pod is terminated, all of its successors must be completely shutdown
-   Each Pod gets an unique name (unique DNS record) derived from the index to indentify pods across the cluster

## YAML Configuration

YAML Configuration is exactly the same as on Deployments, just another `kind` and it requires a `serviceName` which connects to an [HeadlessService](headless-service.md).

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  serviceName: headless-service-name    # Name of required headless service
  podManagementPolicy: Parallel         # If there is no need to scale Pods in order but you want the benefit of unique names for Pods
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

## Storage in StatefulSets

By default, if using [StorageClass](../../storage/storageclass.md) and [PVCs](../../storage/persistentvolumeclaim.md) all Pods will share one Volume. But in some cases (like in the database replica example explained above) each Pod should have it's own Volume. That would require to create a own PVC for each Replica, which is not managable.

For this, StatefulSets supports an option `volumeClaimTemplates`, to define a template for a PVC which then will be used to create a own PVC for each Pod.

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  serviceName: headless-service-name    # Name of required headless service
  podManagementPolicy: Parallel         # If there is no need to scale Pods in order but you want the benefit of unique names for Pods
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
   volumeClaimTemplates:
   - metadata:
       name: www
     spec:
       accessModes: [ "ReadWriteOnce" ]
       storageClassName: "my-storage-class"
       resources:
         requests:
           storage: 1Gi
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create StatefulSet</td><td><strong><code>$ kubectl create -f deployment.yaml</code></strong></td></tr><tr><td>Show all StatefulSets</td><td><strong><code>$ kubectl get statefulset</code></strong></td></tr><tr><td>Show details of Pods</td><td><strong><code>$ kubectl describe statefulset &#x3C;name></code></strong></td></tr><tr><td>Edit StatefulSet</td><td><strong><code>$ kubectl edit statefulset &#x3C;name></code></strong></td></tr><tr><td>Scale StatefulSet</td><td><strong><code>$ kubectl scale statefulset &#x3C;name> --replicas=5</code></strong></td></tr><tr><td>Delete StatefulSet</td><td><strong><code>$ kubectl delete statefulset &#x3C;name></code></strong></td></tr><tr><td>Rollout/Update Deployment</td><td><strong><code>$ kubectl apply -f &#x3C;deployment.yaml></code></strong></td></tr><tr><td>Show rollout history</td><td><strong><code>$ kubectl rollout history deployment &#x3C;deployment name></code></strong></td></tr><tr><td>Rollback</td><td><strong><code>$ kubectl rollout undo deployment/&#x3C;name></code></strong> </td></tr></tbody></table>
