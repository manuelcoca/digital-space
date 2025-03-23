# Application Insights

-   Extension of Azure Monitor providing Application Performance Monitoring
-   Enabled via Auto-Instrumentation (agent) or Application Insights SDK

## Key Features

-   **Live Metrics:** Real-time activity monitoring
-   **Availability:** Synthetic Transaction Monitoring for endpoint testing
-   **GitHub/Azure DevOps Integration:** Create work items from insights
-   **Usage Analysis:** Track user interactions and popular features
-   **Smart Detection:** Automatic failure and anomaly detection
-   **Application Map:** Visual overview of application architecture
-   **Distributed Tracing:** End-to-end visualization of execution flows

## Monitoring Capabilities

-   Request rates, response times, and failure rates
-   Dependency tracking for external services
-   Exception tracking (server and browser)
-   Page views and load performance
-   AJAX calls performance
-   User and session counts
-   Performance counters (CPU, memory, network)
-   Host diagnostics from Docker or Azure
-   Diagnostic trace logs
-   Custom events and metrics

## Metrics Types

-   **Standard-Metrics:** Pre-aggregated for better dashboard performance and real-time alerting
-   **Log-Metrics:** More detailed data for in-depth analysis and diagnostics

## Availability Tests (up to 100 per resource)

-   **URL Ping Test:** Validates endpoint responses
-   **Standard Test:** Similar to URL ping with additional features (SSL validation, HTTP verbs)
-   **Custom TrackAvailability Test:** Custom application tests using the SDK

## Usage Analysis Features

-   **Users:** Track usage patterns, locations, browsers
-   **Funnel:** Analyze step-by-step conversion rates
-   **Impact:** Measure load times effect on conversion
-   **Retention:** Track returning users and goal achievement
-   **Users Flow:** Visualize user navigation patterns

## Implementation

-   Requires instrumentation key for application integration
