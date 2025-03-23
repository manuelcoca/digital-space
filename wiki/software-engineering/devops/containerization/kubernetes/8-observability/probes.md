## Pod Status/Lifecycle

The Pod Status tells us where the pod is in its lifecycle:

-   **`Pending`** - kube-scheduler tries to assign pod to node. If it fails pod will remain in the pending status
-   **`Creating`** - As soon as the pod was assigned to a node, the containers inside the pod will be created
-   **`Running`** - If all containers started successfully

Run **`$ kubectl get pod <name>`** to see the status

## Pod Conditions

Pod conditions are additional informations to the Pod status, which are stored in an array:

-   **`PodScheduled`** (true/false)
-   **`Initialized`** (true/false)
-   **`ContainersReady`** (true/false)
-   **`Ready`** (true/false)

Run **`$ kubectl describe pod <name>`** to see the conditions

## Probes

Probes are diagnostic mechanism for checking if a container is running successfully. If the probe doesn't succeed kubelet can restart the container. Each probe has 3 possible results:

-   **`Success`** - The container passed the diagnostic
-   **`Failure`** - The container failed the diagnostic
-   **`Unkown`** - The diagnostic failed, so no action should be taken

## Types of Probes

1. ReadinessProbe
    - Indicates if the applications in a container is ready to service requests
2. LivenessProbe
    - Indicates if the applications in a container is running (activated after startup probe)
3. StartupProbe
    - Indicates successfull pod start (deactivated after first success)

## ReadinessProbe

### The issue with the condition `Ready`

As soon as the containers are created Kubernetes sets the condition to ready and assumes that the application is ready to serve user traffic. But often applications inside the containers takes more time to get ready which will result in Kubernetes directing user traffic to a non-ready application.

### Check if application is ready with Readiness Probes

Readiness Probes can be used to check if the application is ready to service requests by running tests. Only if the tests succeeded the container status will be set to **`Ready`**.

Example use-cases:

-   Web Application - HTTP Request to web server
-   Database - Check if the database TCP port is listening
-   Or run custom scripts which returns a success message if application is ready

### YAML Configurations for ReadinessProbe

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: webapp
spec:
    containers:
        - name: webapp
          image: webapp
          ports:
              - containerPort: 8080
          readinessProbe:
              httpGet:
                  path: /api/ready
                  port: 8080
              initialDelaySeconds: 30 # Probe will run after 30s
              periodSeconds: 5 # Run probe every 5s
              successThreshold: 2 # How often probe should succeed
              failureThreshold: 5 # Set who often probe can fail. By default, if the application is not not ready after three attempts, the probe will stop.
```

```yaml
---
readinessProbe:
    tcpSockert:
        port: 3306
```

```yaml
---
readinessProbe:
    exec:
        command:
            - cat
            - /app/is_ready
```

## LivenessProbe

### The issue with the condition `Running`

Usually, if an process inside a container fails the Container will crash and Kubernetes will restart the containers. However, in practice it can happen that the container is running, but the application inside is not running e.g due to an infinite loop bug in the code.

### Check if application is really running with LivenessProbe

The LivenessProbe can be used to periodically test whether the application is actually healthy. If the test fails, the container is considered unhealhty and is destroyed and recreated.

Example use-cases (same as on ReadinessProbe):

-   Web Application - Periodic HTTP Request to web server
-   Database - Periodic check if the database TCP port is listening
-   Or run custom scripts which returns a success message if application is ready

### YAML Configurations for LivenessProbe

Same as ReadinessProbe:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: webapp
spec:
    containers:
        - name: webapp
          image: webapp
          ports:
              - containerPort: 8080
          livenessProbe:
              httpGet:
                  path: /api/ready
                  port: 8080
              initialDelaySeconds: 30 # Probe will run after 30s
              periodSeconds: 5 # Run probe every 5s
              successThreshold: 2 # How often probe should succeed
              failureThreshold: 5 # Set who often probe can fail. By default, if the application is not not ready after three attempts, the probe will stop.
```

```yaml
---
livenessProbe:
    tcpSockert:
        port: 3306
```

```yaml
---
livenessProbe:
    exec:
        command:
            - cat
            - /app/is_ready
```
