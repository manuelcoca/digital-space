# Hosting Plans

The hosting plan you choose dictates the following behaviors:

* How your function app is scaled.
* The resources available to each function app instance.
* Support for advanced functionality, such as Azure Virtual Network connectivity.

There are 5 plans available:

<table><thead><tr><th width="213">Plan</th><th>Benefits</th></tr></thead><tbody><tr><td><strong>Consumption plan (Serverless)</strong></td><td>Default hosting plan. It scales automatically and you only pay for compute resources when your functions are running. Instances of the Functions host are dynamically added and removed based on the number of incoming events.</td></tr><tr><td><strong>Premium plan</strong></td><td>Automatically scales based on demand using pre-warmed workers, which run applications with no delay after being idle. Isolated network by connecting to virtual networks. Ideal for continuous workloads</td></tr><tr><td><strong>Dedicated plan</strong></td><td>Run your functions within an App Service plan at regular App Service plan rates. Best for long-running scenarios where <a href="https://learn.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview">Durable Functions</a> can't be used. Good for predictive autoscaling and costs.</td></tr><tr><td><strong>App Service Environemnt (ASE)</strong></td><td>Fully isolated and dedicated environment suitable for workloads that need large SKUs or need to co-locate Web Apps and Function.</td></tr><tr><td><strong>Kubernetes</strong></td><td>Kubernetes provides a fully isolated and dedicated environment running on top of the Kubernetes platform.</td></tr></tbody></table>
