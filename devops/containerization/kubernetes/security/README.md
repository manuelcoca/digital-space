# Security

It is important to secure the access to the Kubernetes Cluster and `kube-apiserver`.

* Who can access? (Authentification)
* What can they do? (Authorization)
* Kubernetes components communication between each other is TLS encrypted
* Communication between applications within the cluster can be secured with Network Policies. By default they can all communicate to each other
