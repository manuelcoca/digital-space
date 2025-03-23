Other than ConfigMap, Secrets are used to store sensitive informations like:

-   Passwords
-   SSH keys Values are not stored in plain text instead in a Base64 encoded format. Encoding and Decoding needs to be done be the user:

```
// Encode
$ echo -n 'value to encode' | base64 # Copy output and add to the secret yaml

// Decode
$ echo -n 'value to decode' | base64 --decode # Decode secret to use it
```

-   Secrets are not encrypted. Only encoded in Base64
-   Secrets are not encrypted in the Kubernetes ETCD database
    -   Encryption needs to be implemented: [https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)
    -   Or consider 3rd-party secret store providers like AWS-Provider or Azure-Provider
-   Anyone who can create Pods and Deployments in the Namespace can also access the Secrets
    -   Restrict access to Secrets with RBAC

## YAML Configuration

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: my-secrets
type: kubernetes.io/basic-auth
data:
    USERNAME: !udefd # Values are Base64 encoded


    PASSWORD: dqwfqf # Values are Base64 encoded
```

## Types

Types vary in terms of the validations performed and the constraints Kubernetes imposes on them. If no type is defined, Kubernetes create a Secret of type Opaque. See above how to define types.

| `Opaque`                              | arbitrary user-defined data             |
| ------------------------------------- | --------------------------------------- |
| `kubernetes.io/service-account-token` | ServiceAccount token                    |
| `kubernetes.io/dockercfg`             | serialized `~/.dockercfg` file          |
| `kubernetes.io/dockerconfigjson`      | serialized `~/.docker/config.json` file |
| `kubernetes.io/basic-auth`            | credentials for basic authentication    |
| `kubernetes.io/ssh-auth`              | credentials for SSH authentication      |
| `kubernetes.io/tls`                   | data for a TLS client or server         |
| `bootstrap.kubernetes.io/token`       | bootstrap token data                    |

## Using Secrets

Secrets can be used by pods like [ConfigMap](configmap.md):

### valueFrom

Only import single values by key (not importing all env variables from ConfigMap):

```yaml
---
env:
    - name: ENV_VAR_NAME # Define the environment variable
      valueFrom:
          secretKeyRef:
              name: <secretName>
              key: <key>
```

### envFrom

Importing all environment variables from ConfigMap and replacing the env spec with envFrom:

```yaml
---
envFrom:
    - secretRef:
          name: <secret-name>
```

## Security concerns

Anyone with the base64 encoded secret can easily decode it. As such the secrets can be considered as not very safe.

The concept of safety of the Secrets is a bit confusing in Kubernetes. The [kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/secret) page and a lot of blogs out there refer to secrets as a "safer option" to store sensitive data. They are safer than storing in plain text as they reduce the risk of accidentally exposing passwords and other sensitive data. In my opinion it's not the secret itself that is safe, it is the practices around it.

Secrets are not encrypted, so it is not safer in that sense. However, some best practices around using secrets make it safer. As in best practices like:

-   Not checking-in secret object definition files to source code repositories.
-   [Enabling Encryption at Rest ](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)for Secrets so they are stored encrypted in ETCD.

Also the way kubernetes handles secrets. Such as:

-   A secret is only sent to a node if a pod on that node requires it.
-   Kubelet stores the secret into a tmpfs so that the secret is not written to disk storage.
-   Once the Pod that depends on the secret is deleted, kubelet will delete its local copy of the secret data as well.

Read about the [protections ](https://kubernetes.io/docs/concepts/configuration/secret/#protections)and [risks](https://kubernetes.io/docs/concepts/configuration/secret/#risks) of using secrets [here](https://kubernetes.io/docs/concepts/configuration/secret/#risks)\

Having said that, there are other better ways of handling sensitive data like passwords in Kubernetes, such as using tools like Helm Secrets, [HashiCorp Vault](https://www.vaultproject.io/). I hope to make a lecture on these in the future.

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create Secret with username &#x26; password (imperative)</td><td><p><strong><code>$ kubectl create secret generic &#x3C;name></code></strong> </p><p><strong><code>--from-literal=USERNAME=admin</code></strong> </p><p><strong><code>--from-literal=PASSWORD=1234</code></strong></p></td></tr><tr><td>Create Secret</td><td><strong><code>$ kubectl create -f secret.yaml</code></strong></td></tr><tr><td>Delete Secret</td><td><strong><code>$ kubectl delete secret &#x3C;name></code></strong></td></tr><tr><td>Update Secret</td><td><strong><code>$ kubectl apply -f secret.yaml</code></strong></td></tr><tr><td>Show all Secrets</td><td><strong><code>$ kubectl get secrets</code></strong></td></tr><tr><td>Show environment variables of the ConfigMap</td><td><strong><code>$ kubectl describe secret &#x3C;name></code></strong></td></tr></tbody></table>
