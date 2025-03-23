# ACI - Azure Container Instance

Azure Container Instances (ACI) offers the fastest and simplest way to run a container in Azure, without having to manage any virtual machines and without having to adopt a higher-level service.

> [!NOTE]
> ACI is not recommended for production. For production it's recommended to use App Service as as it supports advanced futures like scaling, backups and more

## Use-cases

-   Run simple isolated applications
-   Task automation
-   Build Jobs

## Features

-   Fast startup. Containers can be started in seconds without the need to provision VMs
-   Simple exposing of containers by IP address or Domain Name
-   Simple manage utilization like CPU cores and memory
-   Persisten Storage - Mount Azure Files shares directly to a container to persist state

## Container groups

The top-level resource in Azure Container Instances is the _container group_. A container group is a collection of containers that get scheduled on the same host machine. The containers in a container group share a lifecycle, resources, local network, and storage volumes. It's similar in concept to a _pod_ in Kubernetes.

With multi-container groups patterns like Sidecar-Pattern can be implemented.

> [!NOTE]
> Multi-container groups only support Linux containers.

### Deployment

There are 2 ways of deploying multi-container groups:

1. Use Resource Manager template (recommended if you need to deploy additional Azure serivces)
2. YAML file (recommended if deploying only container instances)

### Networking

Container groups share an IP address and a port on that IP address. To enable external clients to reach a container within the group, you must expose the port on the IP address and from the container. Containers within a group can reach each other via localhost on the ports that they've exposed, even if those ports aren't exposed externally on the group's IP address.

## Creating Containers

Containers can be created via Azure Portal or Azure CLI. An image can be choosen from either:

-   Public Docker Registry or
-   Self hosted Azure Container Registry

### Azure CLI

<pre><code>% az group create --name az204-aci-rg --location &#x3C;myLocation>
<strong>$ az container create --resource-group az204-aci-rg 
</strong>    --name mycontainer 
    --image mcr.microsoft.com/azuredocs/aci-helloworld 
    --ports 80 
    --dns-name-label &#x3C;DNS_NAME> --location &#x3C;myLocation>
    
# Check if container is running
<strong>$ az container show --resource-group az204-aci-rg 
</strong>    --name mycontainer 
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" 
    --out table 
</code></pre>

Or create a container from a YAML definition [file](env-variables-and-secrets.md) (see here):

```shell
az container create --resource-group myResourceGroup \
    --file container.yaml \
```
