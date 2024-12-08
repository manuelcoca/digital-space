# Azure Event Hub

Azure Event Hubs represents the "front door" for an event pipeline, often called an _event ingestor_ in solution architectures. An event ingestor is a component or service that sits between event publishers and event consumers to decouple the production of an event stream from the consumption of those events.

## Key features

<table><thead><tr><th width="290">Feature</th><th>Description</th></tr></thead><tbody><tr><td>Fully managed PaaS</td><td>Event Hubs is a fully managed Platform-as-a-Service (PaaS) with little configuration or management overhead, so you focus on your business solutions. Event Hubs for Apache Kafka ecosystems give you the PaaS Kafka experience without having to manage, configure, or run your clusters.</td></tr><tr><td>Real-time and batch processing</td><td>Event Hubs uses a partitioned consumer model, enabling multiple applications to process the stream concurrently and letting you control the speed of processing.</td></tr><tr><td>Capture event data</td><td>Capture your data in near-real time in Azure Blob storage or Azure Data Lake Storage for long-term retention or micro-batch processing.</td></tr><tr><td>Scalable</td><td>Scaling options, like Auto-inflate, scale the number of throughput units to meet your usage needs.</td></tr><tr><td>Rich ecosystem</td><td>Event Hubs for Apache Kafka ecosystems enables Apache Kafka (1.0 and later) clients and applications to talk to Event Hubs. You don't need to set up, configure, and manage your own Kafka clusters.</td></tr></tbody></table>

## Key concepts <a href="#key-concepts" id="key-concepts"></a>

Event Hubs contains the following key components:

* An **Event Hubs client** is the primary interface for developers interacting with the Event Hubs client library. There are several different Event Hubs clients, each dedicated to a specific use of Event Hubs, such as publishing or consuming events.
* An **Event Hubs producer** is a type of client that serves as a source of telemetry data, diagnostics information, usage logs, or other log data.
* An **Event Hubs consumer** is a type of client that reads information from the Event Hubs and allows processing of it. Processing may involve aggregation, complex computation and filtering. Processing may also involve distribution or storage of the information in a raw or transformed fashion.
* A **partition** is an ordered sequence of events that is held in an Event Hubs. Partitions are a means of data organization associated with the parallelism required by event consumers. Azure Event Hubs provides message streaming through a partitioned consumer pattern in which each consumer only reads a specific partition of the message stream. As newer events arrive, they're added to the end of this sequence. The number of partitions is specified at the time an Event Hubs is created and can't be changed (default 4 partitions).
* A **consumer group** is a view of an entire Event Hubs. Consumer groups enable multiple consuming applications to each have a separate view of the event stream, and to read the stream independently at their own pace and from their own position. There can be at most five concurrent readers on a partition per consumer group; however it's recommended that there's only one active consumer for a given partition and consumer group pairing. Each active reader receives all of the events from its partition; if there are multiple readers on the same partition, then they'll receive duplicate events.
* **Event receivers**: Any entity that reads event data from an Event Hubs. All Event Hubs consumers connect via the AMQP 1.0 session. The Event Hubs service delivers events through a session as they become available. All Kafka consumers connect via the Kafka protocol 1.0 and later.
* **Throughput units** or **processing units**: Prepurchased units of capacity that control the throughput capacity of Event Hubs.

<figure><img src="../../../../.gitbook/assets/event-hubs-stream-processing.png" alt=""><figcaption></figcaption></figure>

## Event Hub Capture

Azure Event Hubs enables you to automatically capture the streaming data in Event Hubs in an Azure Blob storage or Azure Data Lake Storage account.

* Azure Blob Storage
* Azure Data Lake Storage Gen 1
* Azure Data Lake Storage Gen 2

## Event consumer client <a href="#event-processor-or-consumer-client" id="event-processor-or-consumer-client"></a>

You don't need to build your own solution to meet these requirements. The Azure Event Hubs SDKs provide this functionality. In .NET or Java SDKs, you use an event processor client (`EventProcessorClient`), and in Python and JavaScript SDKs, you use `EventHubConsumerClient`.

## Checkpointing <a href="#checkpointing" id="checkpointing"></a>

_Checkpointing_ is a process by which an event processor marks or commits the position of the last successfully processed event within a partition. Marking a checkpoint is typically done within the function that processes the events and occurs on a per-partition basis within a consumer group.

If an event processor disconnects from a partition, another instance can resume processing the partition at the checkpoint that was previously committed by the last processor of that partition in that consumer group.
