NetworkPolicy is an object to configure ingress and egress traffic rules.

> [!NOTE]
> Incoming traffic is called ingress. Outgoing traffic is called egress.

By default Kubernetes is configured with an "All Allow" rule that allows traffic from any pod to any other pod or services within the cluster.

But sometimes it's required to block communication between specific pods. Example:

-   Only allow traffic to the database from the API server but not from the web application

![[k8s_network_policy.png]]

## YAML Config

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: db-policy
spec:
    podSelector:
        matchLabels:
            role: db # Pod for which network policy should be applied
    policyTypes: # Only Ingress traffic is restricted
        - Ingress
    ingress:
        - from:
              - podSelector: # Allow traffic by labeled Pod "api-pod"
                    matchLabels:
                        name: api-pod
              - namespaceSelector: # Or allow traffic from namespace "prod"
                    matchLabels:
                        name: prod
          ports: # Ports to allow traffic on
              - protocol: TCP
                port: 3306
```

In order to use AND logic in the selectors remove the list within `from`:

```yaml
---
ingress:
    - from:
          - podSelector: # Only allow traffic by labeled Pod "api-pod"
                matchLabels:
                    name: api-pod
            namespaceSelector: # AND from namespace "prod"
                matchLabels:
                    name: prod
```

To define both ingress and egress rules:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: db-policy
spec:
    podSelector:
        matchLabels:
            role: db # Pod for which network policy should be applied
    policyTypes: # Restrict Ingress and Egress traffic
        - Ingress
        - Egress
    ingress:
        - from:
              - podSelector: # Allow traffic by labeled Pod "api-pod"
                    matchLabels:
                        name: api-pod
          ports: # Ports to allow traffic on
              - protocol: TCP
                port: 3306
    egress:
        - to:
              - podSelector: # Only allow egress traffic to pods labeled api-pod
                    matchLabels:
                        name: api-pod
```
