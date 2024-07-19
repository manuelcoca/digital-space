# Storage Queues

The Queue service contains the following components:

![Image showing components of the queue service](https://learn.microsoft.com/en-gb/training/wwl-azure/discover-azure-message-queue/media/queue-storage-service-components.png)

* **URL format:** Queues are addressable using the URL format `https://<storage account>.queue.core.windows.net/<queue>`. For example, the following URL addresses a queue in the diagram above `https://myaccount.queue.core.windows.net/images-to-download`
* **Storage account:** All access to Azure Storage is done through a storage account.
* **Queue:** A queue contains a set of messages. All messages must be in a queue. The queue name must be all lowercase.
* **Message:** A message, in any format, of up to 64 KB. For version 2017-07-29 or later, the maximum time-to-live can be any positive number, or -1 indicating that the message doesn't expire. If this parameter is omitted, the default time-to-live is seven days.
