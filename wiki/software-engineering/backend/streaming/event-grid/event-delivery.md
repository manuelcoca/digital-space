Event Grid provides scheduling and retry policies to control the delivery of events.

It tries to deliver each event at least once for each matching subscription immediately. If a subscriber's endpoint doesn't acknowledge receipt of an event or if there's a failure, Event Grid retries delivery based on a fixed retry schedule and retry policy.

## Retry policy

Retry policies define what to to with events if an endpoint acknowledge errors. By default:

-   **Retry schedule** - Event Grid waits 30 seconds for a response after delivering a message. After 30 seconds, if the endpoint hasn’t responded, the message is queued for retry
    -   If the endpoint responds within 3 minutes, Event Grid attempts to remove the event from the retry queue on a best effort basis but duplicates may still be received.
-   **Maximum number of attempts** - The default value is 30.
-   **Event time-to-live (TTL)** - The default value is 1440 minutes.

If retries doesnt succeed, e.g because of Maximum number of attempts is reached, Event Grid will drop the event or dead-letter the event (read below).

The following table describes the types of endpoints and errors for which retry doesn't happen:

<table><thead><tr><th width="233">Endpoint Type</th><th>Error codes</th></tr></thead><tbody><tr><td>Azure Resources</td><td>400 Bad Request, 413 Request Entity Too Large, 403 Forbidden</td></tr><tr><td>Webhook</td><td>400 Bad Request, 413 Request Entity Too Large, 403 Forbidden, 404 Not Found, 401 Unauthorized</td></tr></tbody></table>

## Dead-letter

Event Grid can send the undelivered event to a storage account. This process is known as **dead-lettering**. Event Grid dead-letters an event when the retry policy (above) fails.
