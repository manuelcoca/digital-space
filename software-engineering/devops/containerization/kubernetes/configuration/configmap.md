# ConfigMap

Environment variables can be defined in a pod definition yaml file. The problem is, that the environment variables gets hard to manage if the amount of pods in the cluster increases.

With ConfigMap, environment variables can be managed centrely.

* ConfigMap helps to decouple environment variables and application configuration from containers and to mount the ConfigMap from outside
* Only for non-confidential data
* Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume

### YAML Configuration

{% code title="configmap.yaml" %}
```
apiVersion: v1
kind: ConfigMap
metadata:
    name: my-config-map
data:
    APP_COLOR: blue
    APP_MODE: prod
```
{% endcode %}

## Using ConfigMaps

### valueFrom

Only import single values by key (not importing all env variables from ConfigMap):

```
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
          env:
            - name: PLAYER_INITIAL_LIVES # Define the environment variable
              valueFrom:
                configMapKeyRef:
                  name: game-demo           # The ConfigMap this value comes from.
                  key: player_initial_lives # The key to fetch.
            - name: UI_PROPERTIES_FILE_NAME
              valueFrom:
                configMapKeyRef:
                  name: game-demo
                  key: ui_properties_file_name    
```

### envFrom

Importing all environment variables from ConfigMap and replacing the env spec with envFrom:

```
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
          envFrom:
              - configMapRef:
                  name: my-config-map
              - configMapRef:                        # Multiple ConfigMaps can be imported
                  name: <another-config-map>         
```

### volumes

ConfigMaps can also be consumed by a file from a volume:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers:
        - name: nginx-container
          image: nginx
          volumeMounts:
          - name: foo
            mountPath: "/etc/foo"
            readOnly: true
    volumes:
    - name: foo
      configMap:
        name: myconfigmap
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create ConfigMap</td><td><strong><code>$ kubectl create -f configmap.yaml</code></strong></td></tr><tr><td>Delete ConfigMap</td><td><strong><code>$ kubectl delete configmap &#x3C;name></code></strong></td></tr><tr><td>Update ConfigMap</td><td><strong><code>$ kubectl apply -f configmap.yaml</code></strong></td></tr><tr><td>Show all ConfigMaps</td><td><strong><code>$ kubectl get configmaps</code></strong></td></tr><tr><td>Show environment variables of the ConfigMap</td><td><strong><code>$ kubectl describe configmap &#x3C;name></code></strong></td></tr></tbody></table>
