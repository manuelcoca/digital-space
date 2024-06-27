# Namespaces

Namespaces provides a mechanism for isolating resources (like pods, deployments etc.) within a single cluster. There are 3 different namespaces by default:

1. Default - In which the user acts by default after a cluster is created
2. kube-system - In which system Pods and Services are running
3. kube-public - In which Resources should be available to all users

## Use-Case

Namespaces are intended for use in environments with many users spread across multiple teams, or large enterprise applications. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. A typical use-case could be:

* Create a namespace for dev and prod environment
* Configuring different Resource limits and policies to those namespaces with [ResourceQuota](../configuration/resources/resourcequota.md)

## DNS

* Namespaces are part of the cluster internal DNS hierarchy. For each new namespace, Kubernetes adds a new DNS entry:
  * `<service-name>.<namespace-name>.svc.cluster.local`
* Names of resources need to be unique within a namespace, but not across namespaces
  * That means, that objects can be adressed by name inside a namespace
  * To address objects in other namespaces, use the full url:&#x20;
    * `<service-name>.<namespace-name>.svc.cluster.local`

## Commands

<table data-header-hidden><thead><tr><th width="244"></th><th></th></tr></thead><tbody><tr><td>Create namespace</td><td><strong><code>$ kubectl create -f namspace.yml</code></strong></td></tr><tr><td>Create namespace with imperative command</td><td><strong><code>$ kubectl create namespace &#x3C;name></code></strong></td></tr><tr><td>Change default namespace</td><td><strong><code>$ kubectl config set-context --current --namespace=prod</code></strong></td></tr></tbody></table>



