# Autoscaling

Depending on the usage of the web app, you can scale the resources of the underlying machine that is hosting your web app up/down. Resources include the number of cores or the amount of RAM available. Scaling out/in is the ability to increase, or decrease, the number of machine instances that are running your web app. To prevent runaway autoscaling, an App Service Plan has an instance limit. Plans in more expensive pricing tiers have a higher limit. Autoscaling can't create more instances than this limit.

{% hint style="info" %}
Not all App Service Plans allow Autoscaling. Read [here](./#app-service-plans).
{% endhint %}

## Autoscale considerations

#### When to Autoscale

1. Improve availability and fault tolerance
   * It can help ensure that client requests to a service won't be denied because of too high workloads

#### When to NOT Autoscale

1. If your web apps perform resource-intensive processing as part of each request, then autoscaling might not be an effective approach. In these situations, manually scaling up may be necessary. For example, if a request sent to a web app involves performing complex processing over a large dataset, depending on the instance size, this single request could exhaust the processing and memory capacity of the instance.
2. Autoscaling isn't the best approach to handling long-term growth. You might have a web app that starts with a few users, but increases in popularity over time. Autoscaling has an overhead associated with monitoring resources and determining whether to trigger a scaling event. In this scenario, if you can anticipate the rate of growth, manually scaling the system over time may be a more cost effective approach.

## How it works

1. Manual scale - Scale by setting the amount of instances manually
2. Autoscale - Autosclae by defining conditions and rules
   * Indicate how to autoscale by creating autoscale conditions. Conditions can have multiple autoscale rules. Autoscaling makes its decisions based on those rules. A rule specifies the threshold for a metric, and triggers an autoscale event (_scale-in_ or _scale-out_) when this threshold is crossed. Example:
     * Scale up > 80% CPU usage
     * Scale down < 80% CPU usage

There are 2 autoscale conditions available:

1. Scale based on a metric
   * CPU Percentage
   * Memory Percentage
   * Disk Queue Length **-** This metric is a measure of the number of outstanding I/O requests across all instances. A high value means that disk contention could be occurring.
   * Http Queue Length - This metric shows how many client requests are waiting for processing by the web app. If this number is large, client requests might fail with HTTP 408 (Timeout) errors.
   * Data In - This metric is the number of bytes received across all instances.
   * Data Out - This metric is the number of bytes sent by all instances.
2. Scale based on a schedule
   * For example scale on a particular time of the day, every day.

## Monitor autoscaling activity

<figure><img src="../../../../.gitbook/assets/autoscale-run-history.png" alt=""><figcaption></figcaption></figure>
