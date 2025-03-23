_initContainers_ can be used to ensure some containers are ready before others in a pod.

-   Pods can have one or more initContainers
-   initContainers are exactly like regular containers, except:
    -   They always run to completion
    -   Each one must complete successfully
-   If an initContainer fail for a pod, Kubernetes restarts the pod

Example use-cases:

-   A process that pulls a code or binary from a repository that will be used by the main web application
-   A process that waits for an external service or database to be up before the actual application starts

## YAML Configuration

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: mypod
spec:
    containers:
        - name: nginx
          image: nginx
    initContainers:
        - name: init-myservice
          image: busybox
          command: ["sh", "-c", "git clone <some-repository-that-will-be-used-by-application> ;"]
```
