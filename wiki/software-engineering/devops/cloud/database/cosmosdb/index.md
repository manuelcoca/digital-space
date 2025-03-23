CosmosDB is a NoSQL database and has some advantages compared to Storage Account tables. It is designed to provide

-   low latency,
-   elastic scalability of throughput,
-   well-defined semantics for data consistency,
-   and high availability

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

-   **Dedicated provisioned throughput mode**: The throughput provisioned on a container is exclusively reserved for that container and it's backed by the SLAs.
-   **Shared provisioned throughput mode**: These containers share the provisioned throughput with the other containers in the same database (excluding containers that have been configured with dedicated provisioned throughput). In other words, the provisioned throughput on the database is shared among all the "shared throughput" containers.

#### Items

Depending on which API is used, an Azure Cosmos DB item can represent either a document in a collection, a row in a table, or a node or edge in a graph. By default, all items that you add to a container are automatically indexed without requiring explicit index or schema management.

## Creating CosmosDB database

First select a database type (once selected, cannot be changed):

-   **Core SQL**: Stores documents in JSON format but uses SQL to query data
-   **Other types** (MongoDB, Cassandra, etc.): Recommended if you already have code that interacts with these database types and want to migrate to Azure

### Key Configuration Areas

#### Basics

-   **Capacity mode**:
    -   **Provisioned throughput**: Good for production and constant traffic
        -   Guarantees throughput/RU's, low latency, and availability
        -   Offers autoscaling, but takes up to 1 hour to scale up/down
        -   May lead to extra costs with unpredictable workloads due to scaling
    -   **Serverless**: Good for peak workloads and unpredictable traffic
        -   Pure consumption model - RU's are consumed only when necessary

#### Geo Redundancy

-   Option to make database redundant (doubles the cost)

#### Networking

-   Similar to other Azure resources (App Service, Function App)
-   Configure public/private access and network security

#### Backup Policy

-   Configure frequency and retention of database backups

#### Encryption

-   Similar to Storage Account encryption options
-   Choose between Microsoft-managed or customer-managed keys

## Creating a CosmosDB Collection

When creating a new container (Data Explorer > New Container), consider:

-   **Database throughput (autoscale)**:
    -   Choose between autoscale or manual throughput rules
-   **Database Max RU/s**:
    -   Configure maximum Request Units per second
    -   Important for guaranteed performance
    -   Determine approximate RU needs (1 KB read = 1 RU)
    -   See [request-units-and-capacity-modes.md](request-units-and-capacity-modes.md) for details
-   **Partition key**:
    -   Critical for performance and costs
    -   Determines how data is physically divided across partitions
    -   More information: [Microsoft Learn - Partitioning Overview](https://learn.microsoft.com/en-us/azure/cosmos-db/partitioning-overview)

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
