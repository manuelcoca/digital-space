Microsoft Azure Service Bus is a fully managed enterprise integration message broker. Service Bus can decouple applications and services. Data is transferred between different applications and services using **messages**.

Some common messaging scenarios are:

-   _Messaging_. Transfer business data, such as sales or purchase orders, journals, or inventory movements.
-   _Decouple applications_. Improve reliability and scalability of applications and services. Client and service don't have to be online at the same time.
-   _Topics and subscriptions_. Enable 1:_n_ relationships between publishers and subscribers.
-   _Message sessions_. Implement workflows that require message ordering or message deferral.

## Service Bus tiers

Service Bus offers a standard and premium tier. The premium tier is recommended for production scenarios. Although the feature sets are nearly identical, these two tiers of Service Bus Messaging are designed to serve different use cases.

Some high-level differences highlighted:

| Premium                               | Standard                       |
| ------------------------------------- | ------------------------------ |
| High throughput                       | Variable throughput            |
| Predictable performance               | Variable latency               |
| Fixed pricing                         | Pay as you go variable pricing |
| Ability to scale workload up and down | N/A                            |
| Message size up to 100 MB             | Message size up to 256 KB      |

## Advanced features <a href="#advanced-features" id="advanced-features"></a>

Service Bus includes advanced features that enable you to solve more complex messaging problems. The following table describes several of these features.

<table><thead><tr><th width="234">Feature</th><th>Description</th></tr></thead><tbody><tr><td>Message sessions</td><td>To create a first-in, first-out (FIFO) guarantee in Service Bus, use sessions. Message sessions enable exclusive, ordered handling of unbounded sequences of related messages.</td></tr><tr><td>Autoforwarding</td><td>The autoforwarding feature chains a queue or subscription to another queue or topic that is in the same namespace.</td></tr><tr><td>Dead-letter queue</td><td>Service Bus supports a dead-letter queue (DLQ). A DLQ holds messages that can't be delivered to any receiver. Service Bus lets you remove messages from the DLQ and inspect them.</td></tr><tr><td>Scheduled delivery</td><td>You can submit messages to a queue or topic for delayed processing. You can schedule a job to become available for processing by a system at a certain time.</td></tr><tr><td>Message deferral</td><td>A queue or subscription client can defer retrieval of a message until a later time. The message remains in the queue or subscription, but it's set aside.</td></tr><tr><td>Batching</td><td>Client-side batching enables a queue or topic client to delay sending a message for a certain period of time.</td></tr><tr><td>Transactions</td><td>A transaction groups two or more operations together into an <em>execution scope</em>. Service Bus supports grouping operations against a single messaging entity within the scope of a single transaction. A message entity can be a queue, topic, or subscription.</td></tr><tr><td>Filtering and actions</td><td>Subscribers can define which messages they want to receive from a topic. These messages are specified in the form of one or more named subscription rules.</td></tr><tr><td>Autodelete on idle</td><td>Autodelete on idle enables you to specify an idle interval after which a queue is automatically deleted. The minimum duration is 5 minutes.</td></tr><tr><td>Duplicate detection</td><td>An error could cause the client to have a doubt about the outcome of a send operation. Duplicate detection enables the sender to resend the same message, or for the queue or topic to discard any duplicate copies.</td></tr><tr><td>Security protocols</td><td>Service Bus supports security protocols such as Shared Access Signatures (SAS), Role Based Access Control (RBAC) and Managed identities for Azure resources.</td></tr><tr><td>Geo-disaster recovery</td><td>When Azure regions or datacenters experience downtime, Geo-disaster recovery enables data processing to continue operating in a different region or datacenter.</td></tr><tr><td>Security</td><td>Service Bus supports standard AMQP 1.0 and HTTP/REST protocols.</td></tr></tbody></table>

## Topics and subscriptions <a href="#topics-and-subscriptions" id="topics-and-subscriptions"></a>

A queue allows processing of a message by a single consumer. In contrast to queues, topics and subscriptions provide a one-to-many form of communication in a publish and subscribe pattern. It's useful for scaling to large numbers of recipients. Each published message is made available to each subscription registered with the topic. Publisher sends a message to a topic and one or more subscribers receive a copy of the message, depending on filter rules set on these subscriptions. The subscriptions can use more filters to restrict the messages that they want to receive.

Publishers send messages to a topic in the same way that they send messages to a queue. But, consumers don't receive messages directly from the topic. Instead, consumers receive messages from subscriptions of the topic.

## Menu/Configuration

<details>

<summary>Queues</summary>

Create multiple Queues inside a Service Bus.

1. **`Max delivery count`**
    - Error count. If someone reads from the queue they need to delete the message from the queue. If this doesn't happen, because of a failure in the code or whatever, the message gets delivered to the next person after a timeout. The **`Max delivery count`** decides how often it should be delivered to a next person
2. **`Message time to live`**
    - How long a message should live
3. **`Lock duration`**
    - Lock duration means how long a message should be logged to read/process it.

</details>
