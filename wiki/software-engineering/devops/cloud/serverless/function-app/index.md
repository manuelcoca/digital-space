Azure Functions lets you develop serverless applications. You can write just the code you need for the problem, without worrying about a whole application or the infrastructure to run it. Functions are often used to implemented a Microservice architecture.

#### Benefits

-   Less codes
-   Maintain less infrastructure
-   Save costs

Azure Functions supports _triggers_, which are ways to start execution of your code, and _bindings_, which are ways to simplify coding for input and output data.

## Function Architecture

A **function** contains two important pieces - your code, which can be written in various languages, and some config, the _function.json_ file. For compiled languages, this config file is generated automatically from annotations in your code. For scripting languages, you must provide the config file yourself.

The _function.json_ file defines the function's trigger, bindings, and other configuration settings. Every function has one and only one trigger. The runtime uses this config file to determine the events to monitor and how to pass data into and return data from a function execution. Following is an example _function.json_ file.

```json
{
	"disabled": false,
	"bindings": [
		// ... bindings here
		{
			"type": "bindingType",
			"direction": "in",
			"name": "myParamName"
			// ... more depending on binding
		}
	]
}
```

The `bindings` property is where you configure both triggers and bindings. Read about [here](triggers-and-bindings.md).

## Folder Structure

The code for all the functions in a specific function app is located in a root project folder that contains a host configuration file. The [host.json](https://learn.microsoft.com/en-us/azure/azure-functions/functions-host-json) file contains runtime-specific configurations and is in the root folder of the function app. A _bin_ folder contains packages and other library files that the function app requires. Specific folder structures required by the function app depend on language:

-   [C# compiled (.csproj)](https://learn.microsoft.com/en-us/azure/azure-functions/functions-dotnet-class-library#functions-class-library-project)
-   [C# script (.csx)](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-csharp#folder-structure)
-   [F# script](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-fsharp#folder-structure)
-   [Java](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-java#folder-structure)
-   [JavaScript](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-node#folder-structure)
-   [Python](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-python#folder-structure)

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
