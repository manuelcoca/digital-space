# Ingress

Ingress helps users access the application using a single URL that can be configured to route traffic to different services within your cluster, based on the URL path. Example:

* https://example.com/path1 -> route to serivce-1
* https://example.com/path2 -> route to serivce-2

## Root Problem

Exposing applications with NodePort Services would require the user to use URL + Port number, which is not suitable for production application.

<div align="left">

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-11 at 22.21.08.png" alt="" width="375"><figcaption></figcaption></figure>

</div>

This problem would require to install a proxy or Cloud Load Balancer between the Cluster and the User, so that the user can access all the services via URL. In production, you often want to route to different services based on the URL path (as described above). This requires to install multipe proxies or Load Balancers which gets hard to maintain (e.g hard to maintain SSL) in production as it scales.

<div align="left">

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-11 at 22.27.48.png" alt="" width="375"><figcaption></figcaption></figure>

</div>

Here Ingress comes in.

## Ingress Basic

In order to solve the above mentioned problems, usually you would

1. Deploy a solution like nginx to route traffic to services&#x20;
   * In Ingress this is done via Ingress Controller
2. Configure solution like SSL, routes etc. (Ingress Resources)
   * In Ingress this is done via Ingress Resources which are define in YAML config

## Ingress Controller

By default, Ingress Controller is not deployed in a Kubernets Cluster. So in order to deploy a Ingress Controller, a Deployment with a supported solution is required. For example following solutions could be used as an Ingress Controller:

* nginx
* Google Cloud Load Balancer
* and more

The Ingress Service still needs to get exposed via an NodePort.

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-12 at 14.17.19.png" alt=""><figcaption></figcaption></figure>

### Nginx Ingress Controller

A few things are necessary to deploy an nginx Ingress Controller:

1. Create an Nginx Ingress Controller Deployment
   * 2 environment variables must be passed that carry the Pod's name and namespace it is deployed to. The nginx service requires these to read the configuration data from within the Pod
   * Ports that ingress controller uses needs to be specified (80 & 443)
2. ConfigMap to store configurations for nginx like, SSL settings, session timeout, logs etc.
3. NodePort Service to expose the Ingress Controller
4. ServiceAccount to allow Ingress Controller check for updates on Ingress Resources
   *   Ingress controllers have additional intelligence built into them to monitor the Kubernetes

       cluster for Ingress resources and configure the underlying nginx server when something is changed. For this it requires a service account with the right set of permissions.

#### 1. Ingress Controller YAML

{% code title="ingress-controller.yaml" %}
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image:  quay.io/kubernetes-ingress- controller/nginx-ingress-controller:0.21.0
          ports:
            - containerPort: 80
      args:
        - /nginx-ingress-controller                           # Run ingress controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration    # Pass ConfigMap object to decouple nginx config. ConfigMap name is nginx-configuration
      env:                                                    # Env variables required for the nginx service
        - name: POD_NAME        
          valueFrom:
            fieldRef:
              fieldPath: metadata.name
        - name: POD_NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace       
       ports:                                                 # Ports that ingress controller uses
         - name: http
           containerPort: 80
         - name: https
           containerPort: 443
```
{% endcode %}

#### 2. ConfigMap YAML

```
apiVersion: v1
kind: ConfigMap
metadata:
    name: nginx-configuration        # Set name according to the name passed in the controller (see above)
```

#### 3. NodePort Service YAML

```
apiVersion: v1
kind: Service
metadata:
    name: online-shop-service
spec:
    type: NodePort
    ports:
      - port: 80
        targetPort: 80
        protocol: TCP
        name: http
      - port: 443
        targetPort: 443
        protocol: TCP
        name: https
    selectors:
      name: nginx-ingress      # Connect Service to the Deployment with label nginx-ingress
```

#### 3. SerivceAccount YAML

```
apiVersion: v1
kind: ServiceAccount
metadata:
    name: nginx-ingress-serviceaccount
```

## Ingress Resources

An Ingress resource is a set of rules and configurations applied on the Ingress controller. Example configurations:

* Route users to services by URL path:
  * https://domain.com/path1 -> route to serivce-1
  * https://domain.com/path2 -> route to serivce-2
* Or route users to services by Domain:
  * https://domain-1.com/ -> route to serivce-1
  * https://domain-2.com/ -> route to serivce-2

### Simple Ingress Resource to route traffic to service

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-online-shop
spec:
    backend:                                # Define where traffic will be routed to. So all requests the nginx controller will receive, will be routed to the Service online-shop-service port 80 
        serviceName: online-shop-service
        servicePort: 80
```

### Rules for advanced routing by path

If no path matches, Kubernetes routes traffic to a default service (which needs to be created manually). Usually a not found page is configured to show if no path matches.&#x20;

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-online-shop
spec:
    rules:
    - http:
        paths:
        - path: /products
          backend:
              serviceName: online-shop-service
              servicePort: 80
        - path: /dashboard
          backend:
              serviceName: dashboard-service
              servicePort: 80
```

### Rules for advanced routing by domain names

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: ingress-online-shop
spec:
    rules:
    - host: domain-01.com 
      http:
        paths:
        - backend:
              serviceName: online-shop-service
              servicePort: 80
    - host: domain-02.com
      http:
        paths:
        - backend:
              serviceName: dashboard-service
              servicePort: 80
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create Ingress Resoruce</td><td><strong><code>$ kubectl create -f ingress.yaml</code></strong></td></tr><tr><td>Show all Ingress Resources</td><td><strong><code>$ kubectl get ingress</code></strong></td></tr><tr><td>Show details of Ingress Resource</td><td><strong><code>$ kubectl describe ingress &#x3C;name></code></strong></td></tr></tbody></table>
