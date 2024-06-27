# Monitor

Azure Monitor is a central service for collecting and managing log files, alerts and metrics from various Azure services like Virtual Machine, Applications, Container etc. Azure Monitor then allows to run queries (e.g with Microsoft Graph) on those data sets.

## Virtual Machine Logging

1. Navigate to Virtual Machine > Diagnostic Settings
2. Install displayed extension&#x20;
   1. The extensions is basically a virtual machine extension that gets installed into the running virtual machine and is able to pull out CPU utilization, disk, network usage and more
3. Configure what Metrics and Logs should be collected

{% hint style="info" %}
Diagnostic Settings for Virtual Machines can also be configured to be collected by Application Insights.
{% endhint %}

## Azure Functions Logging

1. Navigate to Function App > Monitor
2. Enable Monitoring

Function App will use Application Insights by default.

## Menu/Configuration

<details>

<summary>Diagnostic settings</summary>

See all and manage (enable/disabled) resources that are eligible to be used within Azure Monitor. To enable Monitor for a specifc resource, click on the resource and configure **`Diagnostic settings`**:

1. **`Name`**
2. **`Store type`**
   * Archive to a Storage Account - Metrics will be stored in Storage Account. On selecting, the Storage Account needs to be configured
   * Stream to an event hub
   * Send to Log Analytics

</details>
