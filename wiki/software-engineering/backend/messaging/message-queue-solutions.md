Azure supports two types of queue mechanisms: [**Service Bu**](service-bus.md)**s queues** and [**Storage queues**](storage-queues.md).

Service Bus queues are part of a broader Azure messaging infrastructure that supports queuing, publish/subscribe, and more advanced integration patterns. They're designed to integrate applications or application components that may span multiple communication protocols, data contracts, trust domains, or network environments.

Storage queues are part of the Azure Storage infrastructure. They allow you to store large numbers of messages. You access messages from anywhere in the world via authenticated calls using HTTP or HTTPS. A queue message can be up to 64 KB in size. A queue may contain millions of messages, up to the total capacity limit of a storage account.

Queues are commonly used to create a backlog of work to process asynchronously.

## Consider using Service Bus queues <a href="#consider-using-service-bus-queues" id="consider-using-service-bus-queues"></a>

As a solution architect/developer, **you should consider using Service Bus queues** when:

-   Your solution needs to receive messages without having to poll the queue. With Service Bus, you can achieve it by using a long-polling receive operation using the TCP-based protocols that Service Bus supports.
-   Your solution requires the queue to provide a guaranteed first-in-first-out (FIFO) ordered delivery.
-   Your solution needs to support automatic duplicate detection.
-   You want your application to process messages as parallel long-running streams (messages are associated with a stream using the **session ID** property on the message). In this model, each node in the consuming application competes for streams, as opposed to messages. When a stream is given to a consuming node, the node can examine the state of the application stream state using transactions.
-   Your solution requires transactional behavior and atomicity when sending or receiving multiple messages from a queue.
-   Your application handles messages that can exceed 64 KB but won't likely approach the 256-KB limit.
-   SLA - Service Level Agreement. Azure guarantees an availability of 99,99%

## Consider using Storage queues <a href="#consider-using-storage-queues" id="consider-using-storage-queues"></a>

As a solution architect/developer, **you should consider using Storage queues** when:

-   Your application must store over 80 gigabytes of messages in a queue.
-   Your application wants to track progress for processing a message in the queue. It's useful if the worker processing a message crashes. Another worker can then use that information to continue from where the prior worker left off.
-   You require server side logs of all of the transactions executed against your queues.
