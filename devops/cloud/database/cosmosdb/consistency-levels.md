# Consistency Levels

The concept of consistency allows to control the data consistency across replications. You can choose between 4 types:

<figure><img src="../../../../.gitbook/assets/five-consistency-levels.png" alt=""><figcaption></figcaption></figure>

The default consistency level can be configured on the Azure Cosmos DB account at any time. The default consistency level configured on the account applies to all Azure Cosmos DB databases and containers under that account.

{% hint style="info" %}
Azure Cosmos DB guarantees that 100 percent of read requests meet the consistency guarantee for the consistency level chosen
{% endhint %}

1. Strong
   * It guaranteed that when a region writes a piece of data, that all of the other replicated regions are able to read that data at the exact same moment. Data will be synced in the right order. However, compared to the other options, strong consistency needs longer to write all updates to all replicas.
2. Bounded Staleness
   * On this option a leg time can be configured (from days to seconds). This means that data will be available on replications after the configured leg time. Data will be synced in the right order.
3. Session
   * Session is the default type. It keeps the data consistent for a user session. So if a user has an active Session to Region A and B, data will be consistent in Region A and B. Data will be synced in the right order.
4. Consistent Prefix
   *   In consistent prefix, updates made as single document writes see eventual consistency. Updates made as a batch within a transaction, are returned consistent to the transaction in which they were committed. Write operations within a transaction of multiple documents are always visible together.

       Assume two write operations are performed on documents Doc1 and Doc2, within transactions T1 and T2. When client does a read in any replica, the user sees either “Doc1 v1 and Doc2 v1” or “ Doc1 v2 and Doc2 v2”, but never “Doc1 v1 and Doc2 v2” or “Doc1 v2 and Doc2 v1” for the same read or query operation.
5. Eventual
   * The weakest consistency. There is no guarantee the the update comes in the right order. For use-cases where it not necessary to have the order of the data, rather the total amount of updates (e.g likes, or view counts)
