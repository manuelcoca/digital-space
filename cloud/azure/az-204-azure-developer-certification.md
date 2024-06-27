# AZ-204 - Azure Developer Certification

## Links

Learning paths: [https://docs.microsoft.com/en-us/learn/certifications/exams/az-204#two-ways-to-prepare](https://docs.microsoft.com/en-us/learn/certifications/exams/az-204#two-ways-to-prepare)

Azure Code Samples: [https://azure.microsoft.com/en-us/resources/samples/?sort=0](https://azure.microsoft.com/en-us/resources/samples/?sort=0)

[https://home.pearsonvue.com/onvue/system-requirements](https://home.pearsonvue.com/onvue/system-requirements)



* [ ] Pricing tiers
* [ ] Change Feed components
* [ ] Message/Event sizes and Pricing tiers

## Exam questions by topics

### Develop Azure compute solutions (25–30%)

#### Implement containerized solutions&#x20;

1.

#### Implement Azure App Service Web Apps

1. What DNS record is required to link a custom domain name to an Azure App Service so that users can access the app service using the custom domain name? Choose two. Each answer is a complete solution.
   * A Record
   * CNAME Record
2. You have developed a logic app that calls this API, but the problem is that the logic app times out after 120 seconds because the API takes too long to respond. What is the appropriate modification in order to get this to work?
   * Modify to async requests - Azure recommends you use asynchronous requests for long running calls.
3. What is the Azure Powershell (Az Module) command for creating a new Web App?
   * New-AzWebApp
4. What is the maximum number of apps you can install in a single App Service free account?
   * 10
5. You create an Azure web app locally. The web app consists of a ZIP package. You need to deploy the web app by using the Azure CLI. The deployment must reduce the likelihood of locked files. What should you do?
   * Run `az webapp deploy` specifying `–-clean true`.

#### Implement Azure Functions

1. True or false: you can create an Azure Function to run whenever a new email comes into Outlook using it's own native Trigger integration with email
   * False
2. True or false: You can manually trigger a logic app deployed in Azure from Visual Studio
   * True
3. What two methods can you use to restrict public Internet access to a Function App?
   * IP Access restriction
   * Require x-function-key in Header
4. True or false: an Azure Function can have multiple triggers associated with it?
   * False
5. We secured our function against unknown HTTP callers by requiring a function-specific API key be passed with each call. Which of the following fields is the name header in the HTTP requests that needs to contain this key?
   * x-function-key
6. Which of the following is an advantage of using bindings in your Azure Functions to access data sources and data sinks?
   * Less code, simple connection - Bindings provide a declarative way to connect to data. They let you avoid the complexity of coding connection logic like making a database connection or invoking web API interfaces.
7. How many triggers must a function have ?
   * One
8. You plan to create an Azure function app named **app1**. You need to ensure that app1 will satisfy the following requirements: 1. Supports automatic scaling 2. Has event-based scaling behavior 3. Provides a serverless pricing model. Which hosting plan should you use?
   * Consumption&#x20;
9. Which of the following Azure Functions hosting plans is best when predictive scaling and costs are required?
   * Dedicated plans run in App service which supports setting autoscaling rules based on predictive usage.

### Develop for Azure storage (15–20%)

#### Develop solutions that use Azure Cosmos DB&#x20;

1. What is the concept of strong consistency with Cosmos DB?
   * Strong consistency is that, across the world, readers are guaranteed to always get the most recent committed version of an item.
2. What optional service can you enable, for a fee, to protect Azure SQL Database from unusual client behavior and potentially harmful attempts to access or exploit databases?
   * Advanced Threat Protection - ATP (Advanced Threat Protection) will detect attempts to hack your SQL Database.
3. Which TCP network port do you need to allow traffic to pass through so that you can connect to a SQL Server database, by default?
   * 1433
4. What optional security feature can you enable for Azure SQL Database or SQL Server that will ensure data remains encrypted while at rest, during movement between client and server, and while the data is in use?
   * HTTPS only
5. What is the maximum storage capacity of a Cosmos DB container?
   * Unlimited
6. You plan to implement a storage mechanism for managing state across multiple change feed consumers. You need to configure the change feed processor in the .NET SDK for Azure Cosmos DB for NoSQL API. Which component should you use?
   * Lease container
7. You have an application that writes data to Azure Cosmos DB. The application must offer consistent prefix and monotonic reads. You need to configure the consistency level. Which consistency level should you use?
   * Session

#### Develop solutions that use Azure Blob Storage

1. What is the maximum file size of a block blob?
   * 190.7 TB
2. You need to store data in the cloud in a tabular format, but your primiary concern is the cost. You expect the data to be about 100 GB and less than 1 million accesses per month. You want the data storage solution that costs the least, on average. Which Azure table data store would be the best solution in this case?
   * Table Storage - Table Storage is the most cost effective solution for storing data in table format, but does not offer the features and speed of other solutions.
3. 5 PB is quite a large amount of storage, and very few uses are going to fill that up. Assuming that you will not fill it up, what would be the most likely reason you need to create more than one storage account?
   * Exceeding the maxiumun of 20.000 IO per seconds
4. Suppose you have two video files stored as blobs. One of the videos is business-critical and requires a replication policy that creates multiple copies across geographically diverse datacenters. The other video is non-critical, and a local replication policy is sufficient. Which of the following options would satisfy both data diversity and cost sensitivity consideration.
   * Create two storage accounts, because a storage account by itself has no financial cost. The first account makes use of Geo-redundant storage (GRS) and hosts the business-critical video content. The second account makes use of Local-redundant storage (LRS) and hosts the non-critical video content
5. In a typical project, when would you create your storage account(s)?
   * At the beginning, during project setup - Storage accounts are stable for the lifetime of a project. It's common to create them at the start of a project.
6. You can use either the REST API or the Azure client library to programmatically access a storage account. What is the primary advantage of using the client library?
   * Convenience - Code that uses the client library is much shorter and simpler than code that uses the REST API. The client library handles assembling requests and parsing responses for you.
7. Which of the following can be used to initialize the Blob Storage client library within an application?
   * Azure Storage account connection string
8. A company has a blob in the Archive access tier. You need to rehydrate the blob to an online tier. What are two possible ways to achieve this goal? Each correct answer presents a complete solution.
   * Copy the blob to a new blob in the Hot or Cool tier with the Copy Blob operation.
   * Change the blob’s tier using the Set Blob Tier operation.
9. The application uploads a blob named img1.jpg. Snapshot 1 is then created out of the blob. And then Snapshot 2 is created out of the blob. Snapshot 1 is then deleted. A system error has caused the application to now go ahead and delete the blob and all of its snapshots. Would you be able to restore the blob img1.jpg?
   * Yes

### Implement Azure security (20–25%)

#### Implement user authentication and authorization

1. What is the primary difference between the Owner role and the Contributor role?
   * The owner of a resource can grant access to it to others. The contributor can control the resource but not give access to it to others.
2.  You need to implement authentication for the Azure API. You have the following requirements:

    ✑ All API calls must be secure.

    ✑ Callers to the API must not send credentials to the API.

    Which authentication mechanism should you use?

    *   Managed identity

        (Richtig)
3. You manage an Azure Active Directory registered application named **app1**. App1 calls a web API, which then calls Microsoft Graph. You need to ensure the signed-in user identity is delegated through the request chain. Which authentication flow should you use?
   * On-Behalf-Of - OAuth 2.0 On-Behalf-Of flow (OBO) is used when an application invokes a service or web API, which in turn needs to call another service or web API. The idea is to propagate the delegated user identity and permissions through the request chain.

#### Implement secure Azure solutions

1. What is the main benefit of using Privileged Identity Management with Azure AD?
   * Monitoring and protection of superuser accounts, providing a higher level of oversight to those more powerful accounts
2. You develop an App Service app hosted on Windows Platform. Users report that the app is failing. You need to begin troubleshooting the app by inspecting a copy of the page that is returned when the HTTP return code is greater than 400. Which type of log should you review?
   * Detailed error - The detailed error log contains copies of the error pages, produced in response to HTTP codes greater than 400, that would have been sent to clients. These pages are not sent due to security reasons.
3. Which of the following best practices provides the most flexible and secure way to use a service or account shared access signature (SAS)?
   * he most flexible and secure way to use a service or account SAS is to associate the SAS tokens with a stored access policy.
4. A client app requests managed identities for an access token for a given resource. Which of the below is the basis for the token?
   * Service principal

&#x20;

### Monitor, troubleshoot, and optimize Azure solutions (15–20%)

#### Implement caching for solutions

1. Which Azure CDN vendor does NOT offer the capability to serve different static content compared to a mobile device and a full-sized screen?
   * Akamai
2. You have implemented Redis as a caching service and it's going great. You are running on a premium plan, and using the top 53 GB of memory cache. You'd like to increase the memory limit to 106 GB, but Redis does not support that. How can you get more memory when using Azure Redis? Choose the best answer.
   * Impelement the Redis Cluster feature, and add a second shard to double the memory available - Redis Cluster supports up to 10 shards to create 530 GB of memory.
3. A company plans to host a static website that uses a custom domain and Azure Storage in multiple regions. You need to serve website content and minimize latency. What are two possible ways to achieve this goal? Each correct answer presents part of the solution.
   * Upload static content to a storage container named **$web.**
   * Use Azure Content Delivery Network for regional caching.
4. What is the lowest service tier of Azure Cache for Redis recommended for use in production scenarios?
   * Standard
5. Which of the following represents the expire time resolution when applying a time to live (TTL) to a key in Redis?
   * 1-millisecond

#### Troubleshoot solutions by using Application Insights & Monitor

1. Which Azure monitoring service allows you to set alerts to be notified when service incidents and planned maintenance is happening within Azure in regions that may affect you?
   * Azure Service Health
2. Azure Monitor can collect data from any source - not just the built in sources - using what method?
   * Data Collector API
3. Which of the following availability tests is recommended for authentication tests?
   * Custom TrackAvailability
4. Which of the following metric collection types provides near real-time querying and alerting on dimensions of metrics, and more responsive dashboards?
   * Pre-aggregated metrics are stored as a time series and only with key dimensions, which enable near real-time alerting on dimensions of metrics, more responsive dashboards.

### Connect to and consume Azure services and third-party services (15– 20%)

#### Implement API Management

1. A company uses Azure API Management to expose some of its services. Each developer consuming APIs must use a single key to obtain access to various APIs without requiring approval from the API publisher. You need to recommend a solution. Which solution should you recommend?
   * Define a subscription with product scope.
2. Which of the following components of the API Management service would a developer use if they need to create an account and subscribe to get API keys?
   * The Developer portal serves as the main web presence for developers, and is where they can subscribe to get API keys.
3. Which of the following API Management policies would one use if one wants to apply a policy based on a condition?
   * The choose policy applies enclosed policy statements based on the outcome of evaluation of boolean expressions.

#### Develop event-based solutions

1. Which .NET Framework class contains the EventHubClient for working with Event Hub events?
   * Microsoft.ServiceBus.Messaging
2. The speed of an Azure Event Hub is determined by the number of Throughput units you reserve for it. You can set between 1 and 20 throughput units for the Event Hub. How fast does 1 throughput unit represent for data coming in to an Event Hub?
   * 1 MB per second or 1000 events per second (whichever comes first)
3. Applications that publish messages to Azure Event Hub very frequently will get the best performance using Advanced Message Queuing Protocol (AMQP) because it establishes a persistent socket.
   * True - Publishers can use either HTTPS or AMQP. AMQP opens a socket and can send multiple messages over that socket.
4. By default, how many partitions will a new Event Hub have?
   * 4
5. What is the maximum size for a single publication (individual or batch) that is allowed by Azure Event Hub?
   * 1MB
6. You plan to implement event routing in your Azure subscription by using Azure Event Grid. An event is generated each time an Azure resource is deleted. A message corresponding to the event is automatically displayed in an Azure App Service web app you deployed into the same Azure subscription. You create a custom topic. You need to subscribe to the custom topic. What should you do first?
   * Create an endpoint - before subscribing to the custom topic, you need to create an endpoint for event messages
7. You need to capture events streaming from Azure Event Hubs. To which three locations can you capture data? Each correct answer presents a complete solution.
   * Azure Blob Storage
   * Azure Data Lake Storage Gen 1
   * Azure Data Lake Storage Gen 2

#### Develop message-based solutions

1. At the end of each day, the an application sends a message that contains all of the days statistics in JSON format which needs to be read, processed, and posted to the database. Which Azure Service is best for processing this type of data?
   * Service Bus
2. Suppose you're sending a message with Azure Service Bus and you want multiple components to receive it. Which Azure Service Bus exchange feature should you use?
   * Topic
3. True or false: you can add a message to an Azure Service Bus queue that is 2 MB in size.
   * True - An Azure Storage queue message must be smaller than 64 KB. A service bus queue can be up to 256 KB for standard tier, and 100MB for the premium tier.
4. Which of the following advanced features of Azure Service Bus creates a first-in, first-out (FIFO) guarantee?
   * Message sessions
5. In Azure Service Bus messages are durably stored which enables a load-leveling benefit. Which of the following correctly describes the load-leveling benefit relative to a consuming application's performance?
   * Intermediating message producers and consumers with a queue means that the consuming application only has to be able to handle average load instead of peak load.
6. You need to ensure a publisher can send messages into a topic and multiple subscribers can become eligible to consume the messages. Which message routing pattern should you use?
   * multicast request/reply
7.  A software company is developing a software solution. The software is for a food delivery-based company. The software needs to adhere to the following workflow

    \- A driver selects the restaurants for which they will deliver orders.

    \- Orders are sent to all available drivers in an area.

    \- Only orders for the selected restaurants will appear for the driver.

    \- The first driver to accept an order removes it from the list of available orders.

    The application needs to make use of the Azure Service Bus service.

    Which of the following actions would you implement for this requirement ? Choose 3 answers from the options given below

    * Create a single Service Bus Topic
    * Create a single Service Bus Namespace
    * Create a Service Bus Subscriptions for each restaurant
