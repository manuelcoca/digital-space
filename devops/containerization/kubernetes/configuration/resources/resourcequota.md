# ResourceQuota

With `ResourceQuota`, resource consumption per namespace can be limitteed. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of resources that may be consumed in that namespace. Typical use-case:

* When several users or teams share a cluster with a fixed number of nodes, there is a concern that one team could use more than its fair share of resources. With ResourceQuotas, each team gets the same resources.

## YAML-Configuration

<div align="left">

<figure><img src="../../../../../.gitbook/assets/Screenshot 2023-06-05 at 21.03.13.png" alt="" width="290"><figcaption></figcaption></figure>

</div>

### Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create Quota</td><td><strong><code>$ kubectl create -f newquota.yaml</code></strong></td></tr><tr><td>Create Quota with imperative command</td><td><pre><code>kubectl create quota &#x3C;name> --hard=count/deployments.apps=2,count/replicasets.apps=4,count/pods=3,count/secrets=4 --namespace=myspace
</code></pre></td></tr><tr><td>Describe quota and see limits</td><td><strong><code>$ kubectl describe quota --namespace=&#x3C;name></code></strong></td></tr></tbody></table>
