# Azure CosmosDB

CosmosDB is a NoSQL database and has some advantages compared to Storage Account tables. It is designed to provide

* low latency,
* elastic scalability of throughput,
* well-defined semantics for data consistency,
* and high availability

## Global distribution

You can configure your databases to be globally distributed and available in any of the Azure regions. To lower the latency, place the data close to where your users are. Choosing the required regions depends on the global reach of your application and where your users are located.

Azure Cosmos DB internally handles the data replication between regions with consistency level guarantees of the level you've selected.

Running a database in multiple regions worldwide increases the availability of a database. If one region is unavailable, other regions automatically handle application requests. Azure Cosmos DB offers 99.999% read and write availability for multi-region databases.

## Fundamentals

<figure><img src="../../../../../.gitbook/assets/cosmos-entities.png" alt=""><figcaption></figcaption></figure>

#### Database Account

The Azure Cosmos DB account is the fundamental unit and contains a unique DNS name. Currently, you can create a maximum of 50 Azure Cosmos DB accounts under an Azure subscription (this is a soft limit that can be increased via support request).

#### Database

A database is analogous to a namespace. A database is the unit of management for a set of Azure Cosmos DB containers.

#### Container

An Azure Cosmos DB container is the unit of scalability both for provisioned throughput and storage. You can virtually have an unlimited provisioned throughput (RU/s) and storage on a container.

Azure Cosmos DB partitions containers using the partition key in order to scale the provisioned throughput and storage. The items added to the container are automatically grouped into partitions based on the partition key, which are then distributed across different regions (physical partitions). The throughput on a container is evenly distributed across the physical partitions.

Possible throughput modes for containers:

* **Dedicated provisioned throughput mode**: The throughput provisioned on a container is exclusively reserved for that container and it's backed by the SLAs.
* **Shared provisioned throughput mode**: These containers share the provisioned throughput with the other containers in the same database (excluding containers that have been configured with dedicated provisioned throughput). In other words, the provisioned throughput on the database is shared among all the “shared throughput” containers.

#### Items

Depending on which API is used, an Azure Cosmos DB item can represent either a document in a collection, a row in a table, or a node or edge in a graph. By default, all items that you add to a container are automatically indexed without requiring explicit index or schema management.

## Creating CosmosDB database

First select a database type (once selected, cannot be changed):

1. Core SQL
   * Stores documents in JSON format but uses SQL to query data
2. All other types are recommended if you already have code that interacts with the databases like MongoDB, Casandra etc. but you just want to migrate the database to Azure

{% tabs %}
{% tab title="Basics" %}
1) Capacity mode
   * Provisioned throughput - Good for production and for constant traffic. Guarantees throughput/RU's (read/write operations), low latency, availability and autoscaling. The problem with autoscaling is, that it takes up to 1 hour to scale up/down. Peak workloads (RU's will occur unpredictable and high) will lead to scaling the database up and down all the time which will result in unused capacity and extra costs
   * Serverless - Good for peak workloads and if traffic is unpredictable which is a pure consumption model. RU's are consumed only if necessary.
{% endtab %}

{% tab title="Geo Redundancy" %}
Decide if database should be redundant (it will double the cost of the database)
{% endtab %}

{% tab title="Networking" %}
Networking settings similar to other resources (see [App Service](../../serverless/app-service/) or [Function App](../../serverless/function-app/))
{% endtab %}

{% tab title="Backup Policy" %}
Configure Backup Policy for the CosmosDB database
{% endtab %}

{% tab title="Encryption" %}
Same as Encryption configuration for [Storage Account](../../file-storage/storage-account/)
{% endtab %}
{% endtabs %}

## Creating a CosmosDB Collection

Under Data Explorer > New Container. If the capacity Mode is set to Provisioned throughput:

1. **`Database throughput (autoscale)`**
   * Decide if database should autoscale or use manual rules
2. **`Database Max RU/s`**
   * Configuring max. RU/s is necessary so that Azure can guarantee this performance. At this moment, you need to know how much RU's you approximately need (A read of 1 KB is 1 RU). Read more here[request-units-and-capacity-modes.md](request-units-and-capacity-modes.md "mention").
3. **`Partition key`**
   * Basically, partition key is how the Cosmos DB is going to divide your data physically across various partitions of Cosmos DB, very important for performance and costs! Read more: [https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning-overview](https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning-overview)

## Menu/Configuration

<details>

<summary>Replicate data globally</summary>

A Map to select regions where the database should keep replicas of the database

</details>

<details>

<summary>Keys</summary>

Under this section you can find the endpoint URL and keys to access the database

</details>

<details>

<summary>Default consistency</summary>

Configure default consistency levels described [here](consistency-levels.md).

</details>
