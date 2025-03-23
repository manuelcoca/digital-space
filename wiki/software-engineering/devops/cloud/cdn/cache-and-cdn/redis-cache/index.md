Azure Cache for Redis provides an in-memory data store based on the [Redis](https://redis.io/) software. Redis improves the performance and scalability of an application that uses backend data stores heavily. It's able to process large volumes of application requests by keeping frequently accessed data in the server memory, which can be written to and read from quickly. Redis brings a critical low-latency and high-throughput data storage solution to modern applications.

## Use-cases

Azure Cache for Redis improves application performance by supporting common application architecture patterns. Some of the most common include the following patterns:

<table><thead><tr><th width="182">Pattern</th><th>Description</th></tr></thead><tbody><tr><td>Data cache</td><td>Databases are often too large to load directly into a cache. It's common to use the <a href="https://learn.microsoft.com/en-us/azure/architecture/patterns/cache-aside">cache-aside</a> pattern to load data into the cache only as needed. When the system makes changes to the data, the system can also update the cache, which is then distributed to other clients.</td></tr><tr><td>Content cache</td><td>Many web pages are generated from templates that use static content such as headers, footers, banners. These static items shouldn't change often. Using an in-memory cache provides quick access to static content compared to backend datastores.</td></tr><tr><td>Session store</td><td>This pattern is commonly used with shopping carts and other user history data that a web application might associate with user cookies. Storing too much in a cookie can have a negative effect on performance as the cookie size grows and is passed and validated with every request. A typical solution uses the cookie as a key to query the data in a database. Using an in-memory cache, like Azure Cache for Redis, to associate information with a user is faster than interacting with a full relational database.</td></tr><tr><td>Job and message queuing</td><td>Applications often add tasks to a queue when the operations associated with the request take time to execute. Longer running operations are queued to be processed in sequence, often by another server. This method of deferring work is called task queuing.</td></tr><tr><td>Distributed transactions</td><td>Applications sometimes require a series of commands against a backend data-store to execute as a single atomic operation. All commands must succeed, or all must be rolled back to the initial state. Azure Cache for Redis supports executing a batch of commands as a single <a href="https://redis.io/topics/transactions">transaction</a>.</td></tr></tbody></table>

## Service tiers <a href="#service-tiers" id="service-tiers"></a>

Azure Cache for Redis is available in these tiers:

<table><thead><tr><th width="183">Tier</th><th>Description</th></tr></thead><tbody><tr><td>Basic</td><td>An OSS Redis cache running on a single VM. This tier has no service-level agreement (SLA) and is ideal for development/test and noncritical workloads.</td></tr><tr><td>Standard</td><td>An OSS Redis cache running on two VMs in a replicated configuration.</td></tr><tr><td>Premium</td><td>High-performance OSS Redis caches. This tier offers higher throughput, lower latency, better availability, and more features. Premium caches are deployed on more powerful VMs compared to the VMs for Basic or Standard caches.</td></tr><tr><td>Enterprise</td><td>High-performance caches powered by Redis Labs' Redis Enterprise software. This tier supports Redis modules including RediSearch, RedisBloom, and RedisTimeSeries. Also, it offers even higher availability than the Premium tier.</td></tr><tr><td>Enterprise Flash</td><td>Cost-effective large caches powered by Redis Labs' Redis Enterprise software. This tier extends Redis data storage to nonvolatile memory, which is cheaper than DRAM, on a VM. It reduces the overall per-GB memory cost.</td></tr></tbody></table>

## Creating a Redis Cache

Redis cache can be created via Azure portal, the Azure CLI, or Azure PowerShell. Via Azure Portal it can be created similar like other Services. Important things to notice:

1. Always choose a cache instance location to be in the same as the application. Connecting to a cache in a different region can significantly increase latency and reduce reliability.
2. For production, minimum Standard tier is recommended (see above)

## Accessing Redis Cache via Clients

Clients need the host name, port, and an access key for the cache. You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.

-   The host name is the public Internet address of your cache, which was created using the name of the cache. For example, `sportsresults.redis.cache.windows.net`.
-   The access key acts as a password for your cache. There are two keys created: primary and secondary. You can use either key, two are provided in case you need to change the primary key. You can switch all of your clients to the secondary key, and regenerate the primary key. This would block any applications using the original primary key. Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.

### .Net implementation

With .Net the NuGet Package StackExchange.Redis can be used. Read here.

## Accessing the Redis instance <a href="#accessing-the-redis-instance" id="accessing-the-redis-instance"></a>

Redis has a command-line tool for interacting with an Azure Cache for Redis as a client. The tool is available for Windows platforms by downloading the [Redis command-line tools for Windows](https://github.com/MSOpenTech/redis/releases/). If you want to run the command-line tool on another platform, download Azure Cache for Redis from [https://redis.io/download](https://redis.io/download).

Redis supports a set of known commands. A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.

Here are some common commands you can use:

| Command                 | Description                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ping`                  | Ping the server. Returns "PONG".                                                                                                                     |
| `set [key] [value]`     | Sets a key/value in the cache. Returns "OK" on success.                                                                                              |
| `get [key]`             | Gets a value from the cache.                                                                                                                         |
| `exists [key]`          | Returns '1' if the **key** exists in the cache, '0' if it doesn't.                                                                                   |
| `type [key]`            | Returns the type associated to the value for the given **key**.                                                                                      |
| `incr [key]`            | Increment the given value associated with **key** by '1'. The value must be an integer or double value. This returns the new value.                  |
| `incrby [key] [amount]` | Increment the given value associated with **key** by the specified amount. The value must be an integer or double value. This returns the new value. |
| `del [key]`             | Deletes the value associated with the **key**.                                                                                                       |
| `flushdb`               | Delete _all_ keys and values in the database.                                                                                                        |
