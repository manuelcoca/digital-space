A replicaSet is an controller which creates multiple pods, each with the same spec information - called replicas.

-   High availability - ReplicaSets allow high availability. Example: If 3 pods are deployed and one Pod fails, the Replication Controller will create another Pod and ensure that always 3 pods are available
-   Load Balancing - Additional pods can be deployed to manage the load. If you would run out of resources in one node, the Replication Controller can take care of replicas across multiple nodes

![[k8s_replicaset_overview.png]]

## ReplicationController vs. ReplicaSets

ReplicationController is an old implementation and is replaced by ReplicaSets. Both can still be used. The difference is, that for ReplicaSet defining a selector is obligatory. Labels and selectors are necessary for the ReplicaSets to know which Pods in the cluster to manage. The advantage is that with the selectors already created pods can be managed by ReplicaSets. That is not possible with ReplicationController, since it will only manage the Pods defined in the template section. ![[k8s_replicaset_vs_replicationController.png]]

## YAML-Configuration

-   Inside the spec, it expects a pod template. The structure inside the template is same as you would define a pod
-   This example would create 3 Pods called _myapp-pod_ each running with an nginx container: ![[k8s_replicaset_config.png]]

## Scaling

There are 3 ways to scale:

-   Edit the `replicas` property in the yml file and run:
    -   `kubectl replace -f replicaset-definition.yml`
-   Or run command to scale replicas (replicas configured in the yml will not be update!!!):
    -   `kubectl scale -–replicas=6 –f <yml-file>`
    -   `kubectl scale -–replicas=6 replicaset <replicaset-name>`

## Commands

-   Create ReplicaSet: `kubectl create -f <replicaSet.yml>`
-   Get ReplicaSets: `kubectl get replicaset`
-   Delete ReplicaSet and all underlying PODs: `kubectl delete replicaset <replicaSet-name>`
-   Scale by updating config file: `kubectl replace -f replicaset-definition.yml`
-   Scale without updating config file:
    -   `kubectl scale -–replicas=6 –f <yml-file>`
    -   `kubectl scale -–replicas=6 replicaset <replicaset-name>`
-   Edit ReplicaSet: `kubectl edit rs <replicaSet-name>`
-   Describe ReplicaSet: `kubectl describe rs/<replicaSet-name>`
