Taints and tolerations are used to restrict what pods can be assigned to what nodes. By default, when Pods are created the kube-scheduler will assign the Pods to the Nodes equally.

But in case we have dedicated resources on node one for a particular use case, or application we need to restrict, that specific pods are assigned to specific nodes. How it works:

-   Set Taints on Node
-   Set Tolerations on Pod
-   Only pods that can tolerate a taint on a node, will be assigned to that node

## Taint a node

A Taint tells the node to only accpet Pods with certain tolerations. ![[k8s_taints_tolerations.png]]

-   NoSchedule - Pod will not be assigned
-   PreferNoSchedule - System will assign Pod to node, only if it cannot assigne to another node
-   NoExecute - New pods will not be assigned, and existing pods will be removed from node

## Tolerate Pods

Add tolerations as follows:

```yaml
apiVersion: v1
kind: Pod
metadata: mypod
spec:
    containers:
        - name: nginx
          image: nginx
    tolerations:
        - key: "app"
          operator: "Equal"
          value: "myapp"
          effect: "NoSchedule"
```
