Each time, the kubectl command is executed, it will use the current user (from context) and take the credentials/certifiacte from the KubeConfig file to authenticate against the kube-apiserver.

The KubeConfig file is located under **`$HOME/.kube/config`** and consists of 3 sections (clusters, contexts, users):

-   Clusters - For configuring different clusters (like dev, prod etc.)
-   Users - List of users accounts with access to these clusters
-   Contexts - Marry the cluster and user infos together to define which user will be used to access which cluster

## Example

Example, creating a context named Admin@Production which will use the admin user to access the production cluster:

```
apiVersion: v1
kind: Config
current-context: Admin@Production            # Set the default user/context
clusters:
  - name: prod
    cluster:
      certificate-authority: /etc/kubernetes/ca.crt
      server: https://my-kube:6443

contexts:
  - name: Admin@Production
    context:
      cluster: prod                          # Name of cluster to access
      user: admin                            # Name of user to use for authentication
      namespace: finance                     # If swichted to that context, it will automatically set this namespace as default

users:
  - name: admin
    user:
      client-certificate: /etc/kubernetes/pki/users/admin.crt
      client-key: admin.key
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>View KubeConfig</td><td><strong><code>$ kubectl config view</code></strong></td></tr><tr><td>Change context to use different user</td><td><strong><code>$ kubectl config use-context Admin@Production</code></strong></td></tr></tbody></table>
