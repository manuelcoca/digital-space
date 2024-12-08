# Azure Event Grid

Azure Event Grid is a serverless event broker that can be used for publishing (receive) and subscribing (distribute) events. With Event Grid costs can be reduced by eliminating the need for constant polling.

* **Publisher -** Event Grid can receive events by other applications, SaaS services and Azure services.
* **Subscriber** - Events are then distributed by Event Grid to subscriber destinations such as applications, Azure services, or endpoints.

Event Grid has built-in support for events coming from Azure services. To support custom events, custom topics can be implemented.

Filters allow to route specific events to different endpoints, multicast to multiple endpoints, and make sure the events are reliably delivered.

<figure><img src="../../../../.gitbook/assets/functional-model.png" alt=""><figcaption></figcaption></figure>

## Concepts

* **Events** - What happened.
* **Event sources** - Where the event took place.
* **Topics** - The endpoint where publishers send events.
* **Event subscriptions** - The endpoint or built-in mechanism to route events to one ore more handler. Subscriptions are also used by handlers to intelligently filter incoming events.
* **Event handlers** - The app or service reacting to the event.

### Events

Every event has common information like: source of the event, time the event took place, and unique identifier. Every event also has specific information that is only relevant to the specific type of event. For example, an event about a new file being created in Azure Storage has details about the file, such as the `lastTimeModified` value.

An event of size up to 64 KB is covered by General Availability (GA) Service Level Agreement (SLA). The support for an event of size up to 1 MB is currently in preview. Events over 64 KB are charged in 64-KB increments.

### Topics

The Event Grid topic provides an endpoint where the source sends events. The publisher creates the Event Grid topic, and decides whether an event source needs one or more topics. To respond to certain types of events, subscribers decide which topics to subscribe to.

* **System topics** are built-in topics provided by Azure services. You don't see system topics in your Azure subscription because the publisher owns the topics, but you can subscribe to them. To subscribe, you provide information about the resource you want to receive events from. As long as you have access to the resource, you can subscribe to its events.
* **Custom topics** are application and third-party topics. When you create or are assigned access to a custom topic, you see that custom topic in your subscription.

### Event subscriptions

A subscription tells Event Grid which events on a topic you're interested in receiving. When creating the subscription, you provide an endpoint for handling the event. You can filter the events that are sent to the endpoint. You can filter by event type, or subject pattern. Set an expiration for event subscriptions that are only needed for a limited time and you don't want to worry about cleaning up those subscriptions.

### Event handler

From an Event Grid perspective, an event handler is the place where the event is sent. The handler takes some further action to process the event. Event Grid supports several handler types. You can use a supported Azure service or your own webhook as the handler. Depending on the type of handler, Event Grid follows different mechanisms to guarantee the delivery of the event. For HTTP webhook event handlers, the event is retried until the handler returns a status code of `200 – OK`. For Azure Storage Queue, the events are retried until the Queue service successfully processes the message push into the queue.
