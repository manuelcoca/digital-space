# ReplicaSets

A replicaSet is an controller which creates multiple pods, each with the same spec information - called replicas.

* High availability - ReplicaSets allow high availability. Example: If 3 pods are deployed and one Pod fails, the Replication Controller will create another Pod and ensure that always 3 pods are available
* Load Balancing - Additional pods can be deployed to manage the load. If you would run out of resources in one node, the Replication Controller can take care of replicas across multiple nodes

<div align="left">

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-06-02 at 21.49.07.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

## ReplicationController vs. ReplicaSets

ReplicationController is an old implementation and is replaced by ReplicaSets. Both can still be used. The difference is, that for ReplicaSet defining a selector is obligatory. Labels and selectors are necessary for the ReplicaSets to know which Pods in the cluster to manage. The advantage is that with the selectors already created pods can be managed by ReplicaSets. That is not possible with ReplicationController, since it will only manage the Pods defined in the template section. e

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-06-02 at 22.11.38 (1).png" alt=""><figcaption></figcaption></figure>

## YAML-Configuration

* Inside the spec, it expects a pod template. The structure inside the template is same as you would define a pod
* This example would create 3 Pods called _myapp-pod_ each running with an nginx container:

<div align="left">

<figure><img src="../../../../.gitbook/assets/Screenshot 2023-06-04 at 13.36.39.png" alt="" width="504"><figcaption></figcaption></figure>

</div>

## Scaling

There are 3 ways to scale:

* Edit the `replicas` property in the yml file and run:
  * **`$ kubectl replace -f replicaset-definition.yml`**
* Or run command to scale replicas (replicas configured in the yml will not be update!!!):
  * **`$ kubectl scale -–replicas=6 –f <yml-file>`**
  * **`$ kubectl scale -–replicas=6 replicaset <replicaset-name>`**

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create ReplicaSet</td><td><strong><code>$ kubectl create -f &#x3C;replicaSet.yml></code></strong></td></tr><tr><td>Get ReplicaSets</td><td><strong><code>$ kubectl get replicaset</code></strong></td></tr><tr><td>Delete ReplicaSet and all underlying PODs</td><td><strong><code>$ kubectl delete replicaset &#x3C;replicaSet-name></code></strong></td></tr><tr><td>Scale by updating config file</td><td><strong><code>$ kubectl replace -f replicaset-definition.yml</code></strong></td></tr><tr><td>Scale without updating config file</td><td><p><strong><code>$ kubectl scale -–replicas=6 –f &#x3C;yml-file></code></strong></p><p><strong><code>$ kubectl scale -–replicas=6 replicaset &#x3C;replicaset-name></code></strong></p></td></tr><tr><td>Edit ReplicaSet</td><td><strong><code>$ kubectl edit rs &#x3C;replicaSet-name></code></strong></td></tr><tr><td>Describe ReplicaSet</td><td><strong><code>$ kubectl describe rs/&#x3C;replicaSet-name></code></strong></td></tr></tbody></table>
