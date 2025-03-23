By default, apps hosted in App Service are accessible directly through the internet.

There are two main deployment types for Azure App Service:

1. Multitenant App Service: In Free, Shared, Basic, Standard, Premium, PremiumV2, and PremiumV3 pricing SKUs
2. Single-tenant App Service: In Isolated SKU. App Service plan is hosted directly in your Azure virtual network

## Multi-tenant App Service networking features <a href="#multi-tenant-app-service-networking-features" id="multi-tenant-app-service-networking-features"></a>

On a Multi-tenant deployment, there are many different customers in the same App Service scale unit, which means you can't connect the App Service network directly to your network like on Single-tenant deployment.

With inbound and outbund rules the network traffic can be controlled.

| Inbound features     | Outbound features                            |
| -------------------- | -------------------------------------------- |
| App-assigned address | Hybrid Connections                           |
| Access restrictions  | Gateway-required virtual network integration |
| Service endpoints    | Virtual network integration                  |
| Private endpoints    |                                              |

Examples use-cases:

| Inbound use case                                                 | Feature              |
| ---------------------------------------------------------------- | -------------------- |
| Support IP-based SSL needs for your app                          | App-assigned address |
| Support unshared dedicated inbound address for your app          | App-assigned address |
| Restrict access to your app from a set of well-defined addresses | Access restrictions  |
