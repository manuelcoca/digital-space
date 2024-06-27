# Monitoring & Debugging

As an administrator it's necessary to monitor Node- and Pod-Level metrics like:

* Number of Pods
* Performance metrics
* Resource consumption (CPU, Memory)

## Metrics Server

In Kubernetes monitoring is done by the Metrics Server. The Metrics Server doesn't support a full featured built-in monitoring solution.&#x20;

### Advanced Open-Source monitoring solutions

Instead, open source solutions can be used:

* Prometheus
* Elastic Stack
* DataDog
* Dynatrace

### How Metrics Server works

The Metrics Server retrieves metrics from all Nodes and Pods, aggregates them and stores them in memory.&#x20;

{% hint style="info" %}
Note that the server is only storing in memory and not on disk. That means, that historical data can't be viewed
{% endhint %}

The Kubelet component contains a subcomponent called Container Advisor which is responsible for retrieving performance metrics from pods and exposing them through the Kubelet-API to make metrics available for the Metrics Server

### Install Metrics-Server

```
# If minikube is used
$ minikube addons enable metrics-server

# Otherwise
$ git clone https://github.com/kubernetes-incubator/metrics-server.git 
$ kubectl create -f .               # Run command inside cloned folder. Command which will deploy pods, services and roles to enable Metrics Server:
```

### View metrics

```
$ kubectl top node         # View all node performance metrics
$ kubectl top pod          # View all pod performance metrics
```

