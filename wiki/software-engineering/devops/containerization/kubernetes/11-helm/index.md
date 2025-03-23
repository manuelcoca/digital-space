# Helm

Kubernetes doesn't care about the application, instead it only know about the single objects and resources.

Helm is kind of an abstraction and works like a package manager that helps us manage our applications in the Kubernetes cluster without thinking too much about Objects. It helps to install, uninstall, upgrade or rollback whole applications:

-   **`$ helm install wordpress`**
    -   Helm can be configured to install our application with one command. It will create all the objects services etc. which we defined
-   **`$ helm upgrade application`**
    -   Helm allows to easily upgrade whole applications
-   **`$ helm rollback wordpress`**
    -   Helm allows rollback of whole application
-   **`$ helm uninstall wordpress`**
    -   Helm allows to delete all resources/uninstall an application with one command

With helm it's not necessary to micro-manage each Kubernetes object.

## Install Helm

### Linux

```
$ curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
$ sudo apt-get install apt-transport-https --yes
$ echo "deb https://baltocdn.com/helm/stable/debian all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
$ sudo apt-get update
$ sudo apt-get install helm
```

## Helm Charts

Converting Object Yaml configs to template in order to pass configuration values from Helm. So instead of hardcoding e.g resources, pass variables. Then create a values.yaml to define the variables which are then passed to the configs. Those template + the values.yaml form a Helm Chart.

A single Helm Chart is basically an abstraction of an application. It contains all the necessary Template Files (Object YAML configs), the values.yaml and a Chart.yaml which contains informations about the chart itself.

Own charts can be created or existing Charts can be downloaded from the community. In order to install applications, install a chart (see commands section).

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>List releases</td><td><strong><code>$ helm list</code></strong></td></tr><tr><td>Delete Pod</td><td><strong><code>$ helm uninstall &#x3C;release-name></code></strong></td></tr><tr><td>Update Pod</td><td><strong><code>$ helm install &#x3C;release-name> &#x3C;chart-name></code></strong></td></tr></tbody></table>
