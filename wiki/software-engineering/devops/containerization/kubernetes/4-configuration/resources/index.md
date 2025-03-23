By default a container has no limit to the resources it can consume on a node. Resources can only be configured on Container level, not Pod level

-   A resource type has a base unit
    -   CPU - specified in units of cores
    -   Memory - specified in units of bytes

## CPU Units

-   1 CPU unit in Kubernetes is equivalent to:
    -   1 AWS / GCP / Azure vCPU
    -   1 Hyperthread on a bare-metal Intel processor
-   CPU fractional requests are allowed. Examples
    -   0.9 or 900m (milli)
    -   0.8 or 800m
    -   ...
    -   0.1 or 100m
-   If a container tries to exceed the CPU limit, the system will throttle the CPU so it can't use more CPU resources than its limit

## Memory Units

-   Memory can be expressed as
    -   Plain integer (e.g. 128974)
    -   Fixed-point integer suffixes: E, P, T, G, M, K (e.g. 129M). Examples:
        -   1 G = 1,000,000,000 bytes
        -   1 M = 1,000,000 bytes
        -   1 K = 1,00 bytes
    -   Power of two equivalents: Ei, Pi, Ti, Gi, Mi, Ki (e.g 123Mi). Examples:
        -   1 Gi = 1,073,741,824 bytes
        -   1 Mi = 1,048,576 bytes
        -   1 Ki = 1,024 bytes
-   If a container exceed the Memory limit, the Pod will be terminated with an Out-Of-Memory error

## Resource Requests

Resource requests are for guaranteeing resources for a Container. Example if 1 CPU is defined:

-   The kube-scheduler tries to place the pod on a node, it uses these numbers to identify a node which has sufficient amount of resources available in order to guarantee the resources.

```yaml
apiVersion: v1
kind: Pod
metadata: mypod
spec:
    containers:
        - name: nginx
          image: nginx
          resources:
              requests:
                  memory: "500Gi"
                  cpu: "1"
```

## Resource Limits

Resource limits define the maximum amount of resources which can be used by a container

> [!NOTE] Resource limits should only be used if the users can misuse a system like on public available systems.

```yaml
apiVersion: v1
kind: Pod
metadata: mypod
spec:
    containers:
        - name: nginx
          image: nginx
          resources:
              limits:
                  memory: "500Gi"
                  cpu: "1"
```
