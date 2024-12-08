# Azure Function App

Azure Functions lets you develop serverless applications. You can write just the code you need for the problem, without worrying about a whole application or the infrastructure to run it. Functions are often used to implemented a Microservice architecture.

#### Benefits

* Less codes
* Maintain less infrastructure
* Save costs

Azure Functions supports _triggers_, which are ways to start execution of your code, and _bindings_, which are ways to simplify coding for input and output data.

## Function Architecture

A **function** contains two important pieces - your code, which can be written in various languages, and some config, the _function.json_ file. For compiled languages, this config file is generated automatically from annotations in your code. For scripting languages, you must provide the config file yourself.

The _function.json_ file defines the function's trigger, bindings, and other configuration settings. Every function has one and only one trigger. The runtime uses this config file to determine the events to monitor and how to pass data into and return data from a function execution. Following is an example _function.json_ file.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

The `bindings` property is where you configure both triggers and bindings. Read about [here](triggers-and-bindings.md).

## Folder Structure

The code for all the functions in a specific function app is located in a root project folder that contains a host configuration file. The [host.json](https://learn.microsoft.com/en-us/azure/azure-functions/functions-host-json) file contains runtime-specific configurations and is in the root folder of the function app. A _bin_ folder contains packages and other library files that the function app requires. Specific folder structures required by the function app depend on language:

* [C# compiled (.csproj)](https://learn.microsoft.com/en-us/azure/azure-functions/functions-dotnet-class-library#functions-class-library-project)
* [C# script (.csx)](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-csharp#folder-structure)
* [F# script](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-fsharp#folder-structure)
* [Java](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-java#folder-structure)
* [JavaScript](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-node#folder-structure)
* [Python](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-python#folder-structure)

## Create a Function App

A Function App is a group of functions. The creation flow is very similar to creating an [App Service](../app-service/).

{% tabs %}
{% tab title="Basics" %}
1. **`Publish`**
   * Code - Deploy App from Code
   * Docker Container - Deploy App from an Docker Image (it can connect to the [ACR](broken-reference) to select an Docker Image)
2. **`Hosting`**
   * Read [here](hosting-plans.md).
{% endtab %}

{% tab title="Storage" %}
Select an [Azure Storage Account](../../file-storage/storage-account/). A Azure Function requires an Storage Account because Functions rely on Azure Storage for operations such as managing triggers and logging function executions, but some storage accounts don't support queues and tables.

The same storage account used by your function app can also be used by your triggers and bindings to store your application data. However, for storage-intensive operations, you should use a separate storage account.
{% endtab %}

{% tab title="Networking" %}
Azure Functions can be attached to Virtual Networks to cut off traffic from the outside world. Only available for Premium Plans
{% endtab %}
{% endtabs %}

## Menu/Configurations

<details>

<summary>Functions</summary>

Create a Azure Function inside an Function App (group of Azure Functions)

1. **`Template`** - Choosing a trigger template to trigger function execution
   1. HTTP Trigger - Function will be executed whenever it receives an HTTP Request
   2. Timer Trigger - Schedule a function execution with Cron expression
   3. Durable Function HTTP Starter - [Read below](./#durable-functions), triggers an durable function
   4. Durable Functions orchestrator - [Read below](./#durable-functions)
   5. Durable Function activities - [Read below](./#durable-functions)
   6. Azure CosmosDB trigger - Function can be triggerd if configured data is changed. Example use-case: Change notification feeds
2. **`Authorization`**
   1. Anonymous - Function doesn't require any authorization
   2. Function - Requires an Access Key to call function
   3. Admin - Higher level authorization

</details>

## Functions vs. WebJob

Functions use WebJob SDK under the hood but consider following differences:

|                                             | Functions                                                                                                                                                                          | WebJobs                                                                                                                                     |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Serverless app model with automatic scaling | Yes                                                                                                                                                                                | No                                                                                                                                          |
| Develop and test in browser                 | Yes                                                                                                                                                                                | No                                                                                                                                          |
| Pay-per-use pricing                         | Yes                                                                                                                                                                                | No                                                                                                                                          |
| Integration with Logic Apps                 | Yes                                                                                                                                                                                | No                                                                                                                                          |
| Trigger events                              | <p>Timer<br>Azure Storage queues and blobs<br>Azure Service Bus queues and topics<br>Azure Cosmos DB<br>Azure Event Hubs<br>HTTP/WebHook (GitHub<br>Slack)<br>Azure Event Grid</p> | <p>Timer<br>Azure Storage queues and blobs<br>Azure Service Bus queues and topics<br>Azure Cosmos DB<br>Azure Event Hubs<br>File system</p> |

Azure Functions offers more developer productivity than WebJobs does. It also offers more options for programming languages, development environments, Azure service integration, and pricing. For most scenarios, it's the best choice.
