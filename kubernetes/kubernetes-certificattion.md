# Kubernetes Certificattion

## Tipps

1. I would highly recommend that questions be answered based on their weight-age. All the questions have their weight-age displayed. There are some questions with lower weight-age that will consume more time. So its better to come back to these questions once you are done with questions of higher weight-age.
2. Time and speed are critical. 2 hrs yes, but since the format of the exam is hands on its quite important to keep a check on the time. Get as much as practice as you can.
3. Resist the urge to answer the questions sequentially.
4. The notepad is your friend. For questions that you want to come back later and answer, make a note of it in the notepad provided in the exam to note the question number and its weightage. Doing this will help you to prioritize which questions you want to take a look at.
5. If you are from windows background like me, then get some understanding on volume mounts, ssh, linux file systems
6. Read the instructions given before the start of the exam on how to copy and paste from kubernetes.io documentation into the shell.

## Master kubectl (imperative) commands

{% code fullWidth="true" %}
```
$ kubectl run nginx --image=nginx
$ kubectl create deployment --image=nginx nginx --replicas=3
$ kubectl expose pod redis --port=6379 --name redis-service

// Configurations
$ kubectl create secret generic <name> --from-literal=USERNAME=admin --from-literal=PASSWORD=1234
$ kubectl create quota <name> --hard=count/deployments.apps=2,count/replicasets.apps=4,count/pods=3,count/secrets=4 --namespace=myspace
$ kubectl taint nodes mynode taint=blue:NoSchedule
$ kubectl label nodes mynode node=master

// Deployments and rollouts
$ kubectl rollout undo deployment <name>
$ kubectl rollout history deployment <name>
$ kubectl rollout status deployment <name>
$ kubectl apply -f deployment.yaml                            # Simply update file and apply to create a new rollout
$ kubectl scale -–replicas=6 replicaset <replicaset-name>     # Scale ReplicaSets or Deployments

// Logs and Metrics
$ kubectl logs <podname>                                  # dump pod logs (stdout)
$ kubectl logs -l name=myLabel                            # dump pod logs, with label name=myLabel (stdout)
$ kubectl logs <podname> --previous                       # dump pod logs (stdout) for a previous instantiation of a container
$ kubectl logs -f <podname>                               # stream pod logs (stdout)
$ kubectl logs -f <podname> -c <container-name>           # stream pod container logs (stdout, multi-container case)
$ kubectl logs -f -l key=value --all-containers           # stream all pods logs with label name=myLabel (stdout)
$ kubectl top node         # View all node performance metrics
$ kubectl top pod          # View all pod performance metrics

// API and auth
$ kubectl auth can-i create deployments --as <username> -n <namespace>
$ kubectl api-resources --namespaced=true
$ kubectl create serviceaccount <name>
$ kubectl create token <service-account-name>
$ kubectl convert -f <old-file> --output-version apps/v1
$ kubectl describe -n kube-system pod kube-apiserver-controlplane

// Helm
$ helm install <chart>
$ helm upgrade <chart>
$ helm rollback <chart>
$ helm uninstall <chart>
$ helm list
```
{% endcode %}

## Checklist

### Application Design

* [x] Define, build and modify container images
* [x] Understand Jobs and CronJobs
* [x] Understand multi-container Pod Design parttern (e.g sidecar, init and other)
* [x] Utilize persistent and ephemeral volumes

### Application Environment, Configuration and Security

* [ ] Discover and use CRD
* [x] Understanding AuthN, AuthZ and Admission Control
* [x] Understanding and defining resource requirements, limits and quotas
* [x] Understand ConfigMaps
* [x] Create & consume Secrets
* [x] Understand ServiceAccounts
* [x] Understand SecurityContexts

### Application Deployment

* [x] Use Kubernetes primitives to implement common deployment strategies (e.g blue/green or canary)
* [x] Understand Deployments and how to perform rolling updates
* [ ] Use the Helm package manage to deploy existing packages

### Application observability and maintenance

* [ ] Understand API deprecations
* [x] Implement probes and health checks
* [x] Use provided tools to monitor Kubernetes applications
* [x] Utilize container logs
* [x] Debugging Kubernetes

### Service & Networking

* [x] Demonstrate basic understanding of NetworkPolicies
* [x] Provide and troubleshoot access to applications via services
* [x] Use Ingress rules to expose applications
