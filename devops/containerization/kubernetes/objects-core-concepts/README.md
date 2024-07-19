# Objects - Core Concepts

Kubernetes Objects are persistent entities in the Kubernetes system and used to represent the state of the cluster:

* What containers are running
* The resource available to those containers
* etc.

All objects can be defined with a yaml config and they all have 4 top level required fields:

1. `apiVersion` - Kubernetes api version used to create the object
2. `kind` - Type of object
3. `metadata` - Data about the object like name, labels etc.
4. `spec` - Specification section. Passing object specific configuration -> different on each object.

{% code title="pod-example.yaml" %}
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
{% endcode %}

## Object Management

The `kubectl` command-line tool supports several different ways to create and manage Kubernetes objects.

## List of Objects

* [Namespaces](./#namespaces)
* [Pods](../pods/)
  * [initContainer](../pods/initcontainer.md)
* [Configuration](../configuration/)
  * [ConfigMap](../configuration/configmap.md)
  * [Secrets](../configuration/secrets.md)
  * [ResourceQuota](../configuration/resources/resourcequota.md)
* [Controllers](../controllers/)
  * [ReplicaSets](../controllers/replicasets.md)
  * [Deployments](../controllers/deployments.md)
  * [StatefulSets](../controllers/statefulsets/)
  * [DaemonSets](../controllers/daemon-sets.md)
  * [Garbage Collection](../../../../kubernetes/objects-core-concepts/broken-reference/)
  * [Jobs](../controllers/jobs-and-cronjobs.md)
  * [Cron Jobs](../../../../kubernetes/objects-core-concepts/broken-reference/)
* [Storage](../storage/)
  * [Volumes](../storage/volumes.md)
  * [PersistenVolumes](../storage/persistentvolume.md)
  * [StorageClass](../storage/storageclass.md) (Dynamic Privisioning)
* [Networking](../../docker/networking.md)
  * [Services](../networking/services.md)
  * [Ingress](../networking/ingress.md)
* [Security](../security/)
  * [ServiceAccount](../security/serviceaccount.md)
  * [NetworkPolicy](../security/networkpolicy.md)
