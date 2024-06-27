# App Service

Azure App Service is an HTTP-based service (A [PaaS](../cloud-models.md) and abstraction above Azure VMs) for hosting web applications, REST APIs, and mobile back ends. It supports a bunch of tools like CI/CD, Auto-Scaling, Backups etc.&#x20;

{% hint style="info" %}
App Service and Web App is the same.
{% endhint %}

## App Service Plans

An app always runs in an _App Service plan_. An App Service plan defines a set of compute resources for a web app to run. One or more apps can be configured to run on the same computing resources (or in the same App Service plan).

When you create an App Service plan in a certain region (for example, West Europe), a set of compute resources is created for that plan in that region. Whatever apps you put into this App Service plan run on these compute resources as defined by your App Service plan. Each App Service plan defines:

* Operating System (Windows, Linux)
* Region (West US, East US, etc.)
* Number of VM instances
* Size of VM instances (Small, Medium, Large)
* Pricing tier (Free, Basic, Standard, Premium, PremiumV2, PremiumV3, Isolated, IsolatedV2)

### Overview Pricing Tiers

<table><thead><tr><th width="195">Features</th><th width="89">Free</th><th width="98">Basic</th><th width="109">Standard</th><th width="103">Premium</th><th>Isolated</th></tr></thead><tbody><tr><td>Custom Domain</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Auto Scale</td><td>No</td><td>Manual</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Daily Backups</td><td>No</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Deployment Slots</td><td>No</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Zone Redundant</td><td>No</td><td>No</td><td>No</td><td>Not all</td><td>Yes</td></tr><tr><td>Network Isolation</td><td>No</td><td>No</td><td>No</td><td>No</td><td>Yes</td></tr><tr><td>Hardware Isolation</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr></tbody></table>

Those App Service Plans are also categorised in following:

* **Shared compute**: **Free** tier, runs an app on the same Azure VM as other App Service apps, including apps of other customers. These tiers allocate CPU quotas to each app that runs on the shared resources, and the resources can't scale out.
* **Dedicated compute**: The **Basic**, **Standard**, **Premium**, **PremiumV2**, and **PremiumV3** tiers run apps on dedicated Azure VMs. Only apps in the same App Service plan share the same compute resources. The higher the tier, the more VM instances are available to you for scale-out.
* **Isolated**: The **Isolated** and **IsolatedV2** tiers run dedicated Azure VMs on dedicated Azure Virtual Networks. It provides network isolation on top of compute isolation to your apps. It provides the maximum scale-out capabilities.

### Isolate Apps

You can potentially save money by putting multiple apps into one App Service plan. However, since apps in the same App Service plan all share the same compute resources you need to understand the capacity of the existing App Service plan and the expected load for the new app.

Isolate your app into a new App Service plan when:

* The app is resource-intensive.
* You want to scale the app independently from the other apps in the existing plan.
* The app needs resource in a different geographical region.

## Creating App Service/Web App

### Azure Portal

Create a Resource **`Web App`** or **`App Service`**:

{% tabs %}
{% tab title="Basics" %}
1. Choosing a **`Publish`** Type -&#x20;
   * Code - Deploy App from Code
   * Docker Container - Deploy App from an Docker Image (it can connect to the [ACR](../containers/#azure-container-registry-acr) to select an Docker Image)
   * Static Web App - Serve application from static html, css and js files
2. Choosing an **`App Service Plan`** -
   * The App Service Plan is the pricing model as well as the hosting option
   * There are basically 2 hosting options/types
     * Shared spaces - Hardware resources are shared with other customers
     * Isolated spaces - Isolated hardware only for you
   * The **`App Service Plan`** also decides which features and advanced settings like automatic scaling, network settings, backups and TLS configuration will be available&#x20;
     * The **`App Service Plan`** can be changed and scaled up or down later without an application downtime
3. Choosing a **`Zone redundancy`** -
   * If activated, instances of application will be deployed to different physical data centers to avoid the risk that one data center will fail
   * Only available for specific **`App Service Plans`**
{% endtab %}

{% tab title="Docker" %}
This tab is only available if **`Publish`** Type is set to: _Docker Container_

1. Choose Registry and Docker Image to deploy the app from
{% endtab %}

{% tab title="Deployment" %}
Connect to GitHub Account and configure GitHub Actions for Continious Deployment (CD).

{% hint style="info" %}
App Service can also be deployed from Visual Studio Tools
{% endhint %}
{% endtab %}

{% tab title="Networking" %}
{% hint style="info" %}
Premium App Service Plan allows additional network settings
{% endhint %}

* Enable public access: If _On_ App is public available, otherwise only from private azure endpoints (also valid for kudu website)

For Premium Plans:

* **`Virtual Network:`** By enabling Virtual Network the access to the application can be restricted to only be accessible to servers that are within the selected virtual network
{% endtab %}

{% tab title="Tags" %}
Tags can be setted as needed to identify and group the application (like Kubernetes labels)
{% endtab %}
{% endtabs %}

### Azure CLI

For a App Service following steps are necessary

1. Create a Resource Group
2. Create a App Service Plan
3. Create App Service

```
$ az group create --name <group-name> --location <location e.g europe>
$ az appservice plan create -g <group.name> -n <new-plan-name>
$ az webapp create -g <group-name> -p <plan-name> -n <new-webapp-name>
```

Or using webapp up command:

```
// First git clone the code to deploy into a directory and run following command from that directory
$ az webapp up --location eastus --name customwebappname --html
```

## Menu/Configurations

<details>

<summary>Deployment Slots</summary>

Azure supports automated deployment directly from several sources:

* Azure DevOps Services
* GitHub
* Bitbucket

Deployment Slot allow to:

* Simply create different environments like prod, staging & dev
* Use Swap function to deploy e.g from staging into prod
  * If a swap is executed, it can be undone, so the old prod is restored

Read more [here](./#deployment-slots).

</details>

<details>

<summary>Configuration</summary>

1. General Settings
   * **`Always on`** - Keep the app loaded even when there's no traffic. By default, **Always On** isn't enabled and the app is unloaded after 20 minutes without any incoming requests. It's required for continuous WebJobs or for WebJobs that are triggered using a CRON expression.
   * **`HTTPS only`** - Enforce HTTPS
   * **`ARR affinity`** - In a multi-instance deployment, ensure that the client is routed to the same instance for the life of the session. You can set this option to **Off** for stateless applications. If **`On`** session will be stored in a cookie and user will always be sent back to the same instance. ATTENTION: If **`On`** Load Balancing will not work properly
   * **`Remote debugging`** - Allow remote debugging from Visual Studio. After a few hours, this will be set to **`Off`** to avoid security risks. So only set to **`On`** while debugging.
2. Application Settings
   * Used to pass env and configuration variables
   * It overwrites the local config files (e.g appsettings)
   * Recommended to use in production. Can also be used to store secrets since the variables will be encrypted if transfered between Azure and the application (encrypted-at-rest)
     * If users who has access to the App Service within Azure Portal should not see production secrets, an [Azure Key Vault](../key-vault.md) can be used

</details>

<details>

<summary>Certificates</summary>

On this section, certificates need to be uploaded for custom domains to allow HTTPS

</details>

<details>

<summary>Scale up / Scale out</summary>

Read more [here](autoscaling.md).

</details>

<details>

<summary>App Service logs</summary>

1. **`Application Logging`** - Stores application logs into a storage account. If no Storage Account available a new one needs to be created. Application logging turns of itself after 12 hours.
2. **`Web server logging`** - Stores Webserver logs like HTTPS Request, IIS specifc logs etc.

Read more [here](diagnostic-logging.md).

</details>

<details>

<summary>Log stream</summary>

View configured and collected Logs. Read more [here](diagnostic-logging.md).

</details>

##
