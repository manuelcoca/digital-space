# Scheduling

Scheduling is the process of managing the pod-node assignment

A scheduler watches for newly created Pods that have no Node assigned. For every new Pod that the scheduler discovers, the scheduler becomes responsible for finding the best Node for that Pod to run on. Further more you can define your own rules for scheduler to decide where to put pods.

Read more: [https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/)

## Label and assign pods to node

[https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/)
