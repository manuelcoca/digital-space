# NodeAffinity

NodeSelectors does not allow advanced logic or expressions (like OR, AND) for Pod-to-Node assignments. NodeAffinity provides these advanced features.

## NodeAffinity vs. NodeSelectors

Left side show nodeSelectors selecting nodes with the label "Large". Same functionality with NodeAffinity on the right side:

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-09 at 14.35.49.png" alt=""><figcaption></figcaption></figure>

## NodeAffinity Types

The NodeAffinity type defines the behaviour of the kube-scheduler while Pods are assigned.

1. `requiredDuringSchedulingIgnoreDuringExecution`
   * `requiredDuringScheduling` - New created Pods will not be assigned to Node, if there is no node with the defined label
   * `IgnoredDuringExecution` - Existing Pods are ignored
2. `preferredDuringSchedulingIgnoredDuringExecution`
   * `preferredDuringScheduling` - New created Pods will be assigned to any Node, if there is no node with the defined label
   * `IgnoredDuringExecution` - Existing Pods are ignored
3. `requiredDuringSchedulingRequiredDuringExecution`
   * `requiredDuringScheduling` - New created Pods will not be assigned to Node, if there is no node with the defined label
   * `RequiredDuringExecution` - Existing Pods are effected by NodeAffinity Rules and will be removed from node if doesn't matches the rules



