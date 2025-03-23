### Cheatsheet

[https://kubernetes.io/de/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/de/docs/reference/kubectl/cheatsheet/)

### Object Management

Since almost everything is an object in Kubernetes, kubectl supports several ways to create and manage these objects:

1. **Imperative commands** - operates on live objects
    1. `kubectl run...`
    2. `kubectl create deployment nginx...`
2. **Imperative config** - object creation by reuseable configs
    1. `kubectl create -f config.yaml`
3. **Declarative object config** - operations are defined automatically
    1. `kubectl apply -R -f config.yaml`

### Imperative Command with --dry-run

`--dry-run`: If you simply want to test your command, use the `--dry-run=client` option. This will not create the resource. Instead, tell you whether the resource can be created and if your command is right. In combination with:

`-o yaml > my.yaml`: This will generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch. Examples:

```shell
$ kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yaml
$ kubectl create deployment --image=nginx nginx --dry-run -o yaml
$ kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml  # Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
```

### Config and Context

```shell
$ kubectl config set-context --current --namespace=prod    # Change the namespace
```

### Viewing and finding objects

```shell
$ kubectl explain <object>                                         # Get object documentation
$ kubectl get/describe all                                         # Get/Describe all ressources
$ kubectl get/describe <object>                                    # Get/Describe specifc resource
$ kubectl get/describe <object> <name>                             # Get/Describe specifc resource by name
$ kubectl get <object> <name> -o yaml                              # Ouput yaml file of resource
$ kubectl get <object> -o wide                                     # Get detailed output about a resource
$ kubectl get/describe <object> --namespace=<name>                 # Get/Describe resource in a namespace
$ kubectl get/describe <object> --all-namespaces                   # Get/Describe resource in all namespaces
$ kubectl get/describe <object> --selector=key=value               # Get/Describe resource by selector
$ kubectl get <object> --show-labels                               # Show all labels of a resource
$ kubectl get pods -l environment=production,tier=frontend         # Show Pods with label production and frontend
$ kubectl get pods -l 'environment in (production)'                # OR operation seperated by comma
$ kubectl get/describe pods --field-selector=status.phase=Running  # Get/Describe all running pods in the namespace
$ kubectl get events --sort-by=.metadata.creationTimestamp         # Get events sorted by creation timestamp
$ kubectl events --types=Warning                                   # List all warning events
```

### Creating, updating & deleting objects

```shell
$ kubectl create/apply/delete -f ./my-manifest.yaml            # Create/update/delete resource(s)
$ kubectl create/apply/delete -f ./my1.yaml -f ./my2.yaml      # Create/update/delete resources from different files
$ kubectl create/apply/delete -f ./dir                         # Create/update/delete resources from whole directory
$ kubectl create/appy/delete -f https://git.io/vPieo           # Create/update/delete resources from URL
$ kubectl create <object> <name> <parameter>                   # Create object from imperative command

$ kubectl edit/delete <object> <name>                          # Edit/delete object
$ kubectl delete <object>,<object> <name> <name>               # Delete multiple objects
$ kubectl delete <object>,<object> <name> <name> -l key=value  # Delete multiple objects with a specific label
$ kubectl -n <namespace> delete <object> --all                 # Delete all objects in a namespace

$ kubectl replace --force -f ./my.yaml                         # Force replace, delete and re-create the resource
```

### Scaling

```shell
$ kubectl scale -–replicas=6 –f <yml-file>                    # Scale a resource specified in yml to 6
$ kubectl scale -–replicas=6 <object> <name>                  # Scale an object by name
```

### Deployments

```shell
kubectl rollout history deployment/frontend                      # Show rollout history
kubectl rollout undo deployment/frontend                         # Rollback to the previous deployment
$ kubectl rollout undo deployment/frontend --to-revision=2         # Rollback to a specific revision
$ kubectl rollout status -w deployment/frontend                    # Watch rolling update status of "frontend" deployment until completion
$ kubectl rollout restart deployment/frontend                      # Rolling restart of the "frontend" deployment
```

### Pods

```shell
$ kubectl run -i --tty busybox --image=busybox:1.28 -- sh                   # Run pod as interactive shell
$ kubectl run <podname> --image=nginx -n mynamespace                        # Start a single instance of nginx pod in the namespace of mynamespace
$ kubectl run <podname> --image=<image> --dry-run=client -o yaml > pod.yaml # Generate spec for running pod and write it into a file called pod.yaml

$ kubectl logs <podname>                                  # dump pod logs (stdout)
$ kubectl logs -l name=myLabel                            # dump pod logs, with label name=myLabel (stdout)
$ kubectl logs <podname> --previous                       # dump pod logs (stdout) for a previous instantiation of a container
$ kubectl logs <podname> -c <container-name>              # dump pod container logs (stdout, multi-container case)
$ kubectl logs -f <podname>                               # stream pod logs (stdout)
$ kubectl logs -f <podname> -c <container-name>           # stream pod container logs (stdout, multi-container case)
$ kubectl logs -f -l key=value --all-containers           # stream all pods logs with label name=myLabel (stdout)

$ kubectl exec <podname> -- ls /                          # Run command in existing pod (1 container case)
$ kubectl exec --stdin --tty <podname> -- /bin/sh         # Interactive shell access to a running pod (1 container case)
$ kubectl exec <podname> -c <container-name> -- ls /      # Run command in existing pod (multi-container case)
$ kubectl top pod <podname> --containers                  # Show metrics for a given pod and its containers
$ kubectl top pod <podname> --sort-by=cpu                 # Show metrics for a given pod and sort it by 'cpu' or 'memory'

$ kubectl cp /tmp/foo_dir <podname>:/tmp/bar_dir                # Copy /tmp/foo_dir local directory to /tmp/bar_dir in a remote pod in the current namespace
$ kubectl cp /tmp/foo <podname>:/tmp/bar -c <container-name>    # Copy /tmp/foo local file to /tmp/bar in a remote pod in a specific container
$ kubectl cp /tmp/foo <namespace>/<podname>:/tmp/bar            # Copy /tmp/foo local file to /tmp/bar in a remote pod in namespace my-namespace
$ kubectl cp <namespace>/<podname>:/tmp/foo /tmp/bar            # Copy /tmp/foo from a remote pod to /tmp/bar locally
```

### Port Forward

```bash
$ kubectl port-forward my-pod 5000:6000                     # Listen on port 5000 on the local machine and forward to port 6000 on my-pod
$ kubectl port-forward svc/my-service 5000                  # listen on local port 5000 and forward to port 5000 on Service backend
$ kubectl port-forward svc/my-service 5000:my-service-port  # listen on local port 5000 and forward to Service target port with name <my-service-port>
```

### Services

```shell
# Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
$ kubectl expose pod redis --port=6379 --name redis-service     # Automatically uses pod labels.
$ kubectl create service clusterip redis --tcp=6379:6379        # This will not use the pods labels as selectors; instead it will assume selectors as app=redis.

$ kubectl expose <object> <image> --port=<number> --name=<service-name>
```
