# LimitRange

Since Kubernetes doesn't have resource limits defined for containers by default, it can be done with LimitRange objects.&#x20;

LimitRange define default values to be set for containers in pods that are created without a request or limit specified in the pod-definition files.

{% hint style="info" %}
If LimitRange is changed it will only affect new created Pods, not existing Pods.
{% endhint %}

## YAML Configuration

{% code title="cpu-limit-range.yaml" %}
```
apiVersion: v1
kind: LimitRange
metadata:
    name: cpu-limit-range
spec:
    limits:
    - default:
         cpu: 500m
       defaultRequest:
         cpu: 500m
       max:
         cpu: "1"
       min:
         cpu: 100m
       type: Container
```
{% endcode %}

{% code title="memory-limit-range.yaml" %}
```
apiVersion: v1
kind: LimitRange
metadata:
    name: memory-limit-range
spec:
    limits:
    - default:
         memory: 1Gi
       defaultRequest:
         memory: 1Gi
       max:
         memory: 2Gi
       min:
         memory: 500Mi
       type: Container
```
{% endcode %}
