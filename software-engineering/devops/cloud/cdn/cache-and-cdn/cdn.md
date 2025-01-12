# CDN

A content delivery network (CDN) is a distributed network of servers that can efficiently deliver web content to users. CDNs store cached content on edge servers in point-of-presence (POP) locations that are close to end users, to minimize latency.

The benefits of using Azure CDN:

* Better performance and improved user experience for end users, especially when using applications in which multiple round-trips are required to load content.
* Large scaling to better handle instantaneous high loads, such as the start of a product launch event.
* Distribution of user requests and serving of content directly from edge servers so that less traffic is sent to the origin server.

## Azure CDN features <a href="#azure-cdn-features" id="azure-cdn-features"></a>

Azure CDN offers the following key features:

* Dynamic site acceleration
* CDN caching rules
* HTTPS custom domain support
* Azure diagnostics logs
* File compression
* Geo-filtering

## How CDN works

<figure><img src="../../../../../.gitbook/assets/azure-content-delivery-network.png" alt=""><figcaption></figcaption></figure>

1. A user (Alice) requests a file (also called an asset) by using a URL with a special domain name, such as `<endpoint name>.azureedge.net`. This name can be an endpoint hostname or a custom domain. The DNS routes the request to the best performing POP location, which is usually the POP that is geographically closest to the user.
2. If no edge servers in the POP have the file in their cache, the POP requests the file from the origin server. The origin server can be an Azure Web App, Azure Cloud Service, Azure Storage account, or any publicly accessible web server.
3. The origin server returns the file to an edge server in the POP.
4. An edge server in the POP caches the file and returns the file to the original requestor (Alice). The file remains cached on the edge server in the POP until the time-to-live (TTL) specified by its HTTP headers expires. If the origin server didn't specify a TTL, the default TTL is seven days.
5. Additional users can then request the same file by using the same URL that Alice used, and can also be directed to the same POP.
6. If the TTL for the file hasn't expired, the POP edge server returns the file directly from the cache. This process results in a faster, more responsive user experience.

## Creating a CDN

Similar steps like on Redis Cache or other Services.

After a CDN profile is created one or more CDN endpoints can be created. An CDN endpoint represents the URL that the application is going to use to access static cached files.

To organize your CDN endpoints by internet domain, web application, or some other criteria, you can use multiple profiles. Because [Azure CDN pricing](https://azure.microsoft.com/pricing/details/cdn/) is applied at the CDN profile level, you must create multiple CDN profiles if you want to use a mix of pricing tiers.

## Control cache behavior

### Rules

* **Caching rules**. Caching rules can be either global (apply to all content from a specified endpoint) or custom. Custom rules apply to specific paths and file extensions.
* **Query string caching**. Query string caching enables you to configure how Azure CDN responds to a query string. Query string caching has no effect on files that can't be cached.

### Time-To-Live (TTL)

If you publish a website through Azure CDN, the files on that site are cached until their TTL expires. The Cache-Control header contained in the HTTP response from origin server determines the TTL duration.

If you don't set a TTL on a file, Azure CDN sets a default value. However, this default may be overridden if you have set up caching rules in Azure. Default TTL values are as follows:

* Generalized web delivery optimizations: seven days
* Large file optimizations: one day
* Media streaming optimizations: one year

### Content updates

CDN will serve content until TTL is expired. To serve updated content, use version string in the asset URL. This approach causes the CDN to retrieve the new asset immediately. Or purge the cache. The cache can be purged via Azure Portal or Azure CLI:

```
az cdn endpoint purge \
    --content-paths '/css/*' '/js/app.js' \
    --name ContosoEndpoint \
    --profile-name DemoProfile \
    --resource-group ExampleGroup
```
