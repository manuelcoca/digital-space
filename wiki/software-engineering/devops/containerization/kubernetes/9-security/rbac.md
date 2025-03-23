You can have two roles:

1. Role
    - Roles apply to namespaced api-resources
        - Run **`$kubectl api-resources --namespaced=true`** to see a list of resources
2. ClusterRole
    - ClustRoles apply to not namespaced/global api-resources
        - Run **`$kubectl api-resources --namespaced=false`** to see a list of resources

## Setup a role

-   [apiGroups](../kubernets-api/) - Apply the core API group of the resources you want to grant access to
    -   You can find the api-group of a resources with the command: kubectl api-resources
-   Resources - Apply the resources you want to grant access to e.g: configmap, secrets, pods...
-   Verbs - Define the actions e.g: get, watch, list...

![[k8s_rbac.png]]

### Link users to roles - RoleBinding

![[k8s_role_binding.png]]

## Commands

<table data-header-hidden><thead><tr><th width="226"></th><th></th></tr></thead><tbody><tr><td>Get Roles</td><td><strong><code>$ kubectl get roles</code></strong></td></tr><tr><td>Get rolebindings</td><td><strong><code>$ kubectl get rolebindings</code></strong></td></tr><tr><td>Describe</td><td><strong><code>$ kubectl describe roles/rolebindings &#x3C;name></code></strong></td></tr><tr><td>Check if user is allowed to access Resource and Verb</td><td><p><strong><code>$ kubectl auth can-i &#x3C;verb> &#x3C;resource></code></strong></p><p><strong><code>$ kubectl auth can-i create deployments</code></strong></p></td></tr><tr><td>Check if specific user in a specific namespace has access</td><td><strong><code>$ kubectl auth can-i create deployments --as &#x3C;username> -n &#x3C;namespace></code></strong></td></tr><tr><td>See list of api-resources</td><td><strong><code>$ kubectl api-resources --namespaced=true</code></strong></td></tr></tbody></table>
