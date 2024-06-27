# Change feed

Change feed in Azure Cosmos DB is a persistent record of changes to a container in the order they occur. Change feed support in Azure Cosmos DB works by listening to an Azure Cosmos DB container for any changes. It then outputs the sorted list of documents that were changed in the order in which they were modified.

## Change feed and operations

Today, you see all inserts and updates in the change feed. You can't filter the change feed for a specific type of operation. Currently change feed doesn't log delete operations. As a workaround, you can add a soft marker on the items that are being deleted. For example, you can add an attribute in the item called "deleted," set its value to "true," and then set a time-to-live (TTL) value on the item. Setting the TTL ensures that the item is automatically deleted.

## Change feed read models

For reading the change feed either a push-model or pull-model can be used:

1. Push Model
   * The change feed processor pushes work to a client that has business logic for processing this work
2. Pull Model
   * The client has to pull the work from the server/change feed processor

{% hint style="info" %}
It is recommended to use the push model because you won't need to worry about polling the change feed for future changes, storing state for the last processed change, and other benefits.
{% endhint %}

## Read change feed with push model

There are two ways you can read from the change feed with a push model:&#x20;

1. Azure Functions Azure Cosmos DB triggers
2. Change feed processor library

{% hint style="info" %}
Azure Functions uses the change feed processor behind the scenes, so these are both similar ways to read the change feed.
{% endhint %}

### Azure Functions

While creating an Azure Function choose for the template an Azure CosmosDB trigger.

### Change feed processor

The change feed processor is part of the Azure Cosmos DB [.NET V3](https://github.com/Azure/azure-cosmos-dotnet-v3) and [Java V4](https://github.com/Azure/azure-sdk-for-java/tree/main/sdk/cosmos/azure-cosmos) SDKs. It simplifies the process of reading the change feed and distributes the event processing across multiple consumers effectively.&#x20;

There are four main components in the change feed processor:&#x20;

1. **The monitored container**: The monitored container has the data from which the change feed is generated. Any inserts and updates to the monitored container are reflected in the change feed of the container.
2. **The lease container**: The lease container acts as a state storage and manage state across multiple change feed consumers. The lease container can be stored in the same account as the monitored container or in a separate account.
3. **The compute instance**: A compute instance hosts the change feed processor to listen for changes. Depending on the platform, it could be represented by a VM, a kubernetes pod, an Azure App Service instance, an actual physical machine. It has a unique identifier referenced as the instance name throughout this article.
4. **The delegate**: The delegate component can be used to define custom logic to process the changes that the change feed processor reads

The normal life cycle of a **compute**/host instance is:

1. Read the change feed.
2. If there are no changes, sleep for a predefined amount of time
3. If there are changes, send them to the **delegate**.
4. When the **delegate** finishes processing the changes successfully, update the **lease** store with the latest processed point in time and go to #1.

For change feed processor .Net Implementation read [here](.net-implementation.md#change-feed-processor)
