## Hosting plans

<table><thead><tr><th width="193.33333333333331"></th><th width="406">Scale out</th><th>Max # instances</th></tr></thead><tbody><tr><td><strong>Consumption plan</strong></td><td>*Event driven. Scale out automatically, even during periods of high load. Azure Functions infrastructure scales CPU and memory resources by adding more instances of the Functions host, based on the number of incoming trigger events.</td><td>Windows: 200, Linux: 100</td></tr><tr><td><strong>Premium plan</strong></td><td>*, based on the number of events that its functions are triggered on.</td><td>Windows: 100, Linux: 20-100</td></tr><tr><td><strong>Dedicated plan</strong></td><td>Manual/autoscale</td><td>10-20</td></tr><tr><td><strong>ASE</strong></td><td>Manual/autoscale</td><td>100</td></tr><tr><td><strong>Kubernetes</strong></td><td>Event-driven autoscale for Kubernetes clusters using KEDA.</td><td>Varies by cluster</td></tr></tbody></table>

## How scaling works

Azure Functions uses a component called the _scale controller_ to monitor the rate of events and determine whether to scale out or scale in. The scale controller uses heuristics for each trigger type. For example, when you're using an Azure Queue storage trigger, it scales based on the queue length and the age of the oldest queue message.

The unit of scale for Azure Functions is the function app. When the function app is scaled out, more resources are allocated to run multiple instances of the Azure Functions host. Conversely, as compute demand is reduced, the scale controller removes function host instances. The number of instances is eventually "scaled in" to zero when no functions are running within a function app.

<figure><img src="https://learn.microsoft.com/en-gb/training/wwl-azure/explore-azure-functions/media/central-listener.png" alt="" width="563"><figcaption></figcaption></figure>

> [!NOTE]
> After your function app has been idle for a number of minutes, the platform may scale the number of instances on which your app runs in to zero. The next request has the added latency of scaling from zero to one. This latency is referred to as a _cold start_.
