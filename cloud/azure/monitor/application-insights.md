# Application Insights

Application Insights is an extension of Azure Monitor and provides Application Performance Monitoring. Application Insights is enabled through either Auto-Instrumentation (agent) or by adding the Application Insights SDK to your application code (Auto-instrumentation need to be disabled if telemetry data is send via API).

## Feature overview <a href="#application-insights-feature-overview" id="application-insights-feature-overview"></a>

Features include, but not limited to:

| Feature                            | Description                                                                                                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Live Metrics                       | Observe activity from your deployed application in real time with no effect on the host environment.                                                          |
| Availability                       | Also known as “Synthetic Transaction Monitoring”, probe your applications external endpoint(s) to test the overall availability and responsiveness over time. |
| GitHub or Azure DevOps integration | Create GitHub or Azure DevOps work items in context of Application Insights data.                                                                             |
| Usage                              | Understand which features are popular with users and how users interact and use your application                                                              |
| Smart Detection                    | Automatic failure and anomaly detection through proactive telemetry analysis.                                                                                 |
| Application Map                    | A high level top-down view of the application architecture and at-a-glance visual references to component health and responsiveness.                          |
| Distributed Tracing                | Search and visualize an end-to-end flow of a given execution or transaction.                                                                                  |

## What Application Insights monitors <a href="#what-application-insights-monitors" id="what-application-insights-monitors"></a>

Application Insights collects Metrics and application Telemetry data, which describe application activities and health, as well as trace logging data.

* **Request rates, response times, and failure rates** - Find out which pages are most popular, at what times of day, and where your users are. See which pages perform best. If your response times and failure rates go high when there are more requests, then perhaps you have a resourcing problem.
* **Dependency rates, response times, and failure rates** - Find out whether external services are slowing you down.
* **Exceptions** - Analyze the aggregated statistics, or pick specific instances and drill into the stack trace and related requests. Both server and browser exceptions are reported.
* **Page views and load performance** - reported by your users' browsers.
* **AJAX calls** from web pages - rates, response times, and failure rates.
* **User and session counts**.
* **Performance counters** from your Windows or Linux server machines, such as CPU, memory, and network usage.
* **Host diagnostics** from Docker or Azure.
* **Diagnostic trace logs** from your app - so that you can correlate trace events with requests.
* **Custom events and metrics** that you write yourself in the client or server code, to track business events such as items sold or games won.

## Metrics

There are 2 types of metrics in Application Insights:

1. Standard-Metrics (Pre-aggregated metrics)
   * Standard metrics are pre-aggregated during collection, they have better performance at query time and are better choice for dashboarding and in real-time alerting.&#x20;
2. Log-Metrics
   1. More data is collected, which makes them the superior option for data analysis and ad-hoc diagnostics

Developers can use the SDK to send events manually (by writing code that explicitly invokes the SDK) or they can rely on the automatic collection of events from auto-instrumentation.&#x20;

## Availability tests

Application Insights supports setting up recurring tests to monitor availability and responsiveness. Application Insights sends web requests to your application at regular intervals from points around the world.

You can create up to 100 availability tests per Application Insights resource, and there are three types of availability tests:

* [URL ping test (classic)](https://learn.microsoft.com/en-us/azure/azure-monitor/app/monitor-web-app-availability): You can create this test through the portal to validate whether an endpoint is responding and measure performance associated with that response. You can also set custom success criteria coupled with more advanced features, like parsing dependent requests and allowing for retries.
* [Standard test (Preview)](https://learn.microsoft.com/en-us/azure/azure-monitor/app/availability-standard-tests): This single request test is similar to the URL ping test. It includes SSL certificate validity, proactive lifetime check, HTTP request verb (for example `GET`, `HEAD`, or `POST`), custom headers, and custom data associated with your HTTP request.
* [Custom TrackAvailability test](https://learn.microsoft.com/en-us/azure/azure-monitor/app/availability-azure-functions): If you decide to create a custom application to run availability tests, you can use the [TrackAvailability()](https://learn.microsoft.com/en-us/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) method to send the results to Application Insights.

## Usage Analysis

Application Insights provides several features to analyse usage of an application ([https://learn.microsoft.com/en-us/azure/azure-monitor/app/usage-funnels](https://learn.microsoft.com/en-us/azure/azure-monitor/app/usage-overview?tabs=aspnetcore)):

* Users - Find out when people use your web app, what pages they're most interested in, where your users are located, and what browsers and operating systems they use
* Funnel - If you need to know if customers are progressing through the entire process or ending the process at some point (step-by-step conversion rates)
* Impact - Analyzes how load times and other properties influence conversion rates for various parts of your app
* Retention - Helps you analyze how many users return to your app, and how often they perform particular tasks or achieve goals
* Users Flow - The User Flows tool visualizes how users move between the pages and features of your site

## Instrumentation key

To give other applications, especially on-premise applications, access to Application Insights and send telemetry data, an instrumentation key need to be passed&#x20;
