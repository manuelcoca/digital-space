# Headless Service

Although StatefulSets allow unique names for Pods, there is still a Service necessary to connect Pods to each other, especially traffic needs to be send to specific Pods (e.g Master database, see example under [StatefulSets](./)).&#x20;

With a usual Service this is not possible, since a Service of type ClusterIP will load balance and send traffic to all Pods.

A HeadlessService allows to a send traffic to a specific pod.

* In order to create a HeadlessService the `clusterIP` needs to be set to `None`
* All a HeadlessService is doings, is creating DNS records for pods
  * `<podname.headless-servicename.svc.cluster-domain.example>`
* HeadlessSerivces are not supported by Deployments and traffic would be sent randomly to the pods
  * The reason is because deployment assign random names to a pod

## YAML Configuration

```
apiVersion: v1
kind: Service
metadata:
    name: my-headless-service
spec:
    clusterIP: None       # This creates an headless service
    selector:
        app: frontend     # Connect Service to Pod by label selectors
    ports:                # Array of port mappings
    - targetPort: 80     
      port: 80
      nodePort: 30080
    - targetPort: 443
      port: 443
      nodePort: 30443
```

## Connect to Pods

Only if `subdomain` and `host` fields on the Pod definition file are configured, the HeadlessService will create DNS records. Those 2 fields are required for HeadlessServices:&#x20;

{% hint style="info" %}
StatefulSet doesnt require those 2 fields. It will automatically create DNS records by simply connecting to the HeadlessService by the `serviceName` field.
{% endhint %}

```
apiVersion: v1
kind: Pod
metadata: 
  name: mypod
spec:
  containers:
  - name: nginx
    image: nginx
  subdomains: my-headless-service    # Pass name of headless service
  hostname: mypod
```
