Kubernetes provides a simple solution to stream the Containers logs inside a Pod by running following command:

**`$ kubectl logs -f <pod-name> <container-name>`**

The **`<container-name>`** is only necessary, if the pod runs multipe containers inside.
