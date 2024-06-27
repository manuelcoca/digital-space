# NodeSelectors

Sometimes not every node has the same hardware resources so you might want specific Pods and Applications to run on specifc nodes.

With NodeSelectors Pods can be limitted to run only on defined Nodes. NodeSelectors are bascially label selectors which means that the node labels has to match the defined key:value pair in the NodeSelector.

## Label a Node

<figure><img src="../../.gitbook/assets/Screenshot 2023-06-09 at 14.26.50.png" alt=""><figcaption></figcaption></figure>

## YAML Configuration

Assign Pod to node with label "Large":

{% code title="pod.yaml" %}
```
apiVersion: v1
kind: Pod
metadata: mypod
spec:
    containers:
    - name: nginx
      image: nginx
    nodeSelector:
      size: Large
```
{% endcode %}
