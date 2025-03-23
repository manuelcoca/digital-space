From a networking perspective, a Pod can be seen as a virtual machine of a physical host. The network needs to assign a IP addresses to Pods, and needs to provide traffic routes between all Pods on any Node.

There are 3 main networking challenges to solve in kubernetes:

1. Container-to-Container communication (solved by the Pod concept)
2. Pod-to-Pod communication
    - Solved by Services using ClusterIP addresses
3. External-to-Pod communication
    - Solved by Services using NodePort addresses

### ClusterIP & NodePort

A ClusterIP is used for traffic within the cluster.

A NodePort first creates a ClusterIP, then associates a port of the node to that new ClusterIP.

![[k8s_network_services.png]]
