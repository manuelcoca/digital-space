# Networking

From a networking perspective, a Pod can be seen as a virtual machine of a physical host. The network needs to assign a IP addresses to Pods, and needs to provide traffic routes between all Pods on any Node.

There are 3 main networking challenges to solve in kubernetes:

1. Container-to-Container communication (solved by the Pod concept)
2. Pod-to-Pod communication
   1. Solved by Services using ClusterIP addresses
3. External-to-Pod communication
   1. Solved by Services using NodePort addresses

### ClusterIP & NodePort

A ClusterIP is used for traffic within the cluster.

A NodePort first creates a ClusterIP, then associates a port of the node to that new ClusterIP.

<div align="left"><figure><img src="../../../../../.gitbook/assets/Screenshot 2023-05-26 at 12.49.50.png" alt=""><figcaption></figcaption></figure></div>
