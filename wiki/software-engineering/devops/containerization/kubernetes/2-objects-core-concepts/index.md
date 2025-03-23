# Objects - Core Concepts

Kubernetes Objects are persistent entities in the Kubernetes system and used to represent the state of the cluster:

-   What containers are running
-   The resource available to those containers
-   etc.

All objects can be defined with a yaml config and they all have 4 top level required fields:

1. `apiVersion` - Kubernetes api version used to create the object
2. `kind` - Type of object
3. `metadata` - Data about the object like name, labels etc.
4. `spec` - Specification section. Passing object specific configuration -> different on each object.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
```

## Object Management

The `kubectl` command-line tool supports several different ways to create and manage Kubernetes objects.

## List of Objects

-   Namespaces
-   initContainer
-   Configuration
    -   ConfigMap
    -   Secrets
    -   ResourceQuota
-   Controllers
    -   ReplicaSets
    -   Deployments
    -   StatefulSets
    -   DaemonSets
    -   Garbage Collection
    -   Jobs
    -   Cron Jobs
-   Storage
    -   Volumes
    -   PersistenVolumes
    -   StorageClass (Dynamic Privisioning)
-   Networking
    -   Services
    -   Ingress
-   Security
    -   ServiceAccount
    -   NetworkPolicy
