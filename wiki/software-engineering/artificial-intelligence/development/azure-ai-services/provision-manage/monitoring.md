---
tags:
    - azureai
    - azure
    - azuremetrics
    - azureanalytics
    - azuremonitoring
---

## Monitor costs

-   Before deploying, estimate costs with [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator/).
-   Costs depends on:
    -   Azure AI Service API which is used
    -   Region
    -   Pricing tier
-   Cost can be monitored on the **Cost analysis** Dashboard of the specific subscription
    -   Select **Cognitive Search** as service name to see overall cost for AI Services

## Alerts

To create an alert rule for an Azure AI Services resource, select the resource in the Azure portal and on the **Alerts** tab, add a new alert rule. To define the alert rule, you must specify:

-   The *scope* of the alert rule - in other words, the resource you want to monitor.
-   A *condition* that triggers the alert. The specific trigger for the alert is based on a *signal type*, which can be *Activity Log*(an entry in the activity log created by an action performed on the resource, such as regenerating its subscription keys) or *Metric* (a metric threshold such as the number of errors exceeding 10 in an hour).
-   Optional *actions*, such as sending an email to an administrator notifying them of the alert, or running an Azure Logic App to address the issue automatically.
-   _Alert rule details_, such as a name for the alert rule and the resource group in which it should be defined.

## Metrics

In the case of Azure AI Services, Azure Monitor collects metrics relating to endpoint requests, data submitted and returned, errors, and other useful measurements.

Metrics can be viewed on the Metrics tab in the Azure Portal.

## Diagnostic Logging

Diagnostic logging enables you to capture rich operational data for an Azure AI Services resource, which can be used to analyse service usage and troubleshoot problems.

### Log Storage

-   To capture diagnostic logs for an Azure AI Services resource, a destination for log data is required.
-   Azure Event Hubs can be used as a destination for log data, allowing forwarding to a custom telemetry solution or third-party solutions.

Typically, you will use one or both of the following resource types within your Azure subscription for this purpose:

-   **Azure Log Analytics** - a service that enables you to query and visualise log data within the Azure portal.
-   **Azure Storage** - a cloud-based data store that you can use to store log archives (which can be exported for analysis in other tools as needed).

If you intend to archive log data to Azure Storage, create the Azure Storage account in the same region as your Azure AI Services resource.

### Configure diagnostic settings

With your log destinations in place, you can configure diagnostic settings for your Azure AI Services resource. You define diagnostic settings on the **Diagnostic settings** page of the blade for your Azure AI Services resource in the Azure portal. When you add diagnostic settings, you must specify:

-   A name for your diagnostic settings.
-   The categories of log event data that you want to capture.
-   Details of the destinations in which you want to store the log data.

### View Logs

Logs can be viewed in Log Analytics by running queries. It can take an hour or more before diagnostic data starts flowing to the destinations.
