## Docker Security Basics

-   In Docker a process in a container runs as a root by default
    -   User can be changed by passing a non-root user id e.g in the Dockerfile
-   Docker limits the rights of a root user in a container with Linux capabilites - if more or less rights are needed the capabilites can be overwritten
-   Containers runs in the Hosts namespace, but each container has also it's own namespace
    -   The root user on docker host can see all namespaces and processes inside that namespaces and containers
    -   But the root user inside a container can only see processes inside it's own namespace

## Security Context

As capabilities and non-root users can be configured in Docker, the same can be done in Kubernetes with security context. A Security Context can be defined on Pod or Container level:

-   On Pod level, all the containers will inherit the security context
-   If both on Pod and Container level defined, the container security context will overwrite the pods one

## YAML Configuration

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
spec:
    securityContext: # Pod level
        runAsUser: 1000
        runAsGroup: 3000
        fsGroup: 2000
    containers:
        - name: nginx-container
          image: nginx
          securityContext: # On Container level
              runAsUser: 1000
              capabilities:
                  add: ["MAC_ADMIN"] # Capabilities can only be added on container level
```
