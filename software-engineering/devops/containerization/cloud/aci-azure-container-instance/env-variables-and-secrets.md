# Env variables & Secrets

Use a YAML definition file for creating a container and defining environment variables or **`secretValue`**.

{% code title="container.yaml" %}
```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```
{% endcode %}

## Create Container

```
az container create --resource-group myResourceGroup \
    --file container.yaml \
```
