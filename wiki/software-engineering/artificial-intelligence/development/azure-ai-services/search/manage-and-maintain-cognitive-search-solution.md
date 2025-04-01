---
tags: azure, azureai, azureaisearch
---

## Manage Security

Azure Cognitive Security builds on Azure's existing network security. Consider 3 ways to secure the solution:

1. Data Encryption
2. Inbound search requests made by users to your search solution
3. Outbound requests from your search solution to other servers to index documents
4. Restricting access at the document level per user search request

### Data Encryption

Like all Azure services, data is encrypted at rest with service-managed keys. This encryption includes indexes, data sources, synonym maps, skillsets and indexer definitions.

Data in transit is encrypted using standard HTTPS.

Custom subscription keys are supported and can be managed via Azure Key Vault. A benefit of using your own customer-managed keys is that double encryption will be enabled on all objects you use your custom keys on.

> For detailed steps on how to use customer-managed keys for encryption, see [Configure customer-managed keys for data encryption in Azure Cognitive Search](https://learn.microsoft.com/en-us/azure/search/search-security-manage-encryption-keys)

### Authenticate requests

1. Key-based authentication
2. Role-based-access-control (RBAC)

#### Key- based authentication

There are 2 key types:

-   **Admin keys** - grant your write permissions and the right to query system information (_maximum of 2 admin keys can be created per search service_)
-   **Query keys** - grant read permissions and are used by your users or apps to query indexes (_maximum of 50 query keys can be created per search service_)

#### RBAC

The built-in roles you can assign to manage the Azure Cognitive Search service are:

-   **Owner** - Full access to all search resources
-   **Contributor** - Same as above, but without the ability to assign roles or change authorizations
-   **Reader** - View partial service information

If you need a role that can also manage the data plane for example search indexes or data sources, use one of these roles:

-   **Search Service Contributor** - A role for your search service administrators (the same access as the Contributor role above) and the content (indexes, indexers, data sources, and skillsets)
-   **Search Index Data Contributor** - A role for developers or index owners who will import, refresh, or query the documents collection of an index
-   **Search Index Data Reader** - Read-only access role for apps and users who only need to run queries

### Secure inbound traffic

If your search solution can be accessed externally from the internet or apps, you can reduce the attack surface. Azure Cognitive Search lets you restrict access to the public endpoint for free using a firewall to allow access from specific IP addresses. ![[inbound-traffic-through-firewalls-azure-cogntive-search-small.png]]

### Secure outbound traffic

Securing outbound traffic depends on the resources used. Usually it is key-based authentication, database logins or similar. Also firewall can be used to only allow access from Azure Cognitive Search IP-Address.

### Secure data at document-level

You can configure Azure Cognitive Search to restrict the documents someone can search, for example, restrict searching contractual PDFs to people in your legal department.

Controlling who has access at the document level requires you to update each document in your search index. You need to add a new security field to every document that contains the user or group IDs that can access it. The security field needs to be filterable so that you can filter search results on the field.

With this field in place and populated with the allowed user or groups, you can restrict results by adding the `search.in`filter to all your search queries. If you're using HTTP POST requests, the body should look like this:

```
{
   "filter":"security_field/any(g:search.in(g, 'user_id1, group_id1, group_id2'))"
}
```

## Measure and Optimise Performance

### Measure Performance with Diagnostic Setting

First performance need to be measured to be able to improve it. To do so, enabled diagnostic logging using Log Analytics:

-   In the Azure portal inside Cognitive Search, select **Diagnostic settings**.
-   Select **+ Add diagnostic settings**.
-   Give your diagnostic setting a name.
-   Select **allLogs** and **AllMetrics**.
-   Select **Send to Log Analytics workspace**.
-   Choose, or create, your Log Analytics workspace.

It's important to capture this diagnostic information at the search service level. As there are several places where your end-users or apps can see performance issues. ![[possible-performance-issues.png]]

### Check if search service is throttled

Azure Cognitive Search searches and indexes can be throttled. If your users or apps are having their searches throttled, it's captured in Log Analytics with a 503 HTTP response. If your indexes are being throttled, they'll show up as 207 HTTP responses.

In the Azure portal, under **Monitoring**, select **Logs**. In the New Query 1 tab, you can use this query to check if service is throttled:

```
AzureDiagnostics
| where TimeGenerated > ago(7d)
| summarize count() by resultSignature_d
| render barchart
```

### Improve Performance of Queries

Use client tools like Postman or Insomnia to check performance of custom queries. If a slow query is identified, follow these steps to improve performance:

1. Only specify the fields you need to search using the **searchFields** parameter. As more fields require extra processing.
2. Return the smallest number of fields you need to render on your search results page. Returning more data takes more time.
3. Try to avoid partial search terms like prefix search or regular expressions. These kinds of searches are more computationally expensive.
4. Avoid using high skip values. This forces the search engine to retrieve and rank larger volumes of data.
5. Limit using facetable and filterable fields to low cardinality data.
6. Use search functions instead of individual values in filter criteria. For example, you can use `search.in(userid, '123,143,563,121',',')` instead of `$filter=userid eq 123 or userid eq 143 or userid eq 563 or userid eq 121`.

If all steps are not improving query performance, the index can be scaled up (adding more partitions, service tier etc.).

## Manage costs

The Azure pricing calculator is a great tool that allows you to estimate the costs of using any of the Azure services. Use it to create a baseline for your search service needs.

1. Browse to the [Azure Cognitive Search pricing calculator](https://azure.microsoft.com/pricing/details/search/).
2. Choose your region, currency, and hour or monthly pricing.

Try to understand how the service is billed (hourly, per request, per response etc.) in order to estimate properly and understand costs.

In production Costs can then be managed via Microsoft Cost Management which allows to track spendings and create Budgets which can take actions if costs increased over configured Budget.

### Tips to reduce the cost of your search solution

1. Minimize bandwidth costs by using as few regions as possible. Ideally, all the resources should reside in the same region.
2. If you have predictable patterns of indexing new data, consider scaling up inside your search tier. Then scale back down for your regular querying.
3. To keep your search requests and responses inside the Azure datacenter boundary, use an Azure Web App front-end as your search app.
4. Enable enrichment caching if you're using AI enrichment on blob storage.

## Backup options

At present, Azure doesn't offer a formal backup and restore mechanism for Azure Cognitive Search. However, you can build your own tools to back up index definitions as a series of JSON files. Then you can recreate your search indexes using these files.

## Service availability

The Azure Cognitive Search service guarantees availability based on the number of replicas you've:

-   Two replicas guarantee 99.9% availability for your queries
-   Three or more replicas guarantee 99.9% availability for both queries and indexing

Distributing replicas to different Availability Zones (Regions) protect against failure in a region. Try to keep regions as near as possible to users to avoid performance issues.

## Monitoring

Azure Monitor can give you insights into how well your search service is being used and performing. You can also receive alerts to proactively notify you of issues.

When you create your Azure Cognitive Search service, without you doing any other setup, you can see your current search latency, queries per second, and the percentage of throttled queries. This data can be viewed on the **Monitoring** tab of the **Overview** page.

### Use metric to visualise diagnostic data

Under the **Monitoring** section of your search service, select **Metrics**. Now select to add any of these captured metrics:

-   DocumentsProcessedCount
-   SearchLatency
-   SearchQueriesPerSecond
-   SkillExecutionCount
-   ThrottledSearchQueriesPercentage

For example, you could plot search latency against the percentage of throttled queries to see if the responses to queries are affected by throttling.

### Kusto queries

Example of Kusto queries to run against captured log data:

Long-running queries

```
AzureDiagnostics
| project OperationName, resultSignature_d, DurationMs, Query_s, Documents_d, IndexName_s
| where OperationName == "Query.Search"
| sort by DurationMs
```

Indexer status

```
AzureDiagnostics
| project OperationName, Description_s, Documents_d, ResultType, resultSignature_d
| where OperationName == "Indexers.Status"
```

HTTP status codes

```
AzureDiagnostics
| where TimeGenerated > ago(7d)
| summarize count() by resultSignature_d
| render barchart
```

Query rates

```
AzureDiagnostics
| where OperationName == "Query.Search" and TimeGenerated > ago(1d)
| extend MinuteOfDay = substring(TimeGenerated, 0, 16)
| project MinuteOfDay, DurationMs, Documents_d, IndexName_s
| summarize QPM=count(), AvgDuractionMs=avg(DurationMs), AvgDocCountReturned=avg(Documents_d)  by MinuteOfDay
| order by MinuteOfDay desc
| render timechart
```

Average Query Latency

```
let intervalsize = 1m;
let _startTime = datetime('2021-02-23 17:40');
let _endTime = datetime('2021-02-23 18:00');
AzureDiagnostics
| where TimeGenerated between(['_startTime']..['_endTime']) // Time range filtering
| summarize AverageQueryLatency = avgif(DurationMs, OperationName in ("Query.Search", "Query.Suggest", "Query.Lookup", "Query.Autocomplete"))
by bin(TimeGenerated, intervalsize)
| render timechart
```

Average Queries Per Minute (QPM)

```
let intervalsize = 1m;
let _startTime = datetime('2021-02-23 17:40');
let _endTime = datetime('2021-02-23 18:00');
AzureDiagnostics
| where TimeGenerated between(['_startTime'] .. ['_endTime']) // Time range filtering
| summarize QueriesPerMinute=bin(countif(OperationName in ("Query.Search", "Query.Suggest", "Query.Lookup", "Query.Autocomplete"))/(intervalsize/1m), 0.01)
by bin(TimeGenerated, intervalsize)
| render timechart
```

Indexing Operations Per Minute (OPM)

```
let intervalsize = 1m;
let _startTime = datetime('2021-02-23 17:40');
let _endTime = datetime('2021-02-23 18:00');
AzureDiagnostics
| where TimeGenerated between(['_startTime'] .. ['_endTime']) // Time range filtering
| summarize IndexingOperationsPerSecond=bin(countif(OperationName == "Indexing.Index")/ (intervalsize/1m), 0.01)
by bin(TimeGenerated, intervalsize)
| render timechart
```

## Debug search issue

Azure Cognitive Search indexing can be debugged with **Debug Session**. Debug Session is an interactive visual editor.  that lets you step through the enrichment pipeline of a document as it's enriched. You can step into each individual skill, make changes and fixes, and then rerun the indexer in real-time.

### Create a Debug Session

1. Select **Debug Sessions** under Search management in the Overview pane.
2. Select **+ Add Debug Session**.
3. In **Debug session name**, provide a name that will help you remember which skillset, indexer, and data source the debug session is about.
4. In **Storage connection string**, find a general-purpose storage account for caching the debug session.
5. In **Indexer template**, select the indexer that drives the skillset you want to debug. Copies of both the indexer and skillset are used to initialize the session.
6. In **Document to debug**, choose the first document in the index or select a specific document.
7. Select **Save Session** to get started.

### Explore and edit skills

Your Debug Session lets you explore how a document is enriched as it passes through each of the AI skills. You can select a skill, review the inputs and outputs, and even see the JSON definition for the skill.

1. In the dependency graph, select a **skill**. [![A screenshot of the Expression evaluator.](https://learn.microsoft.com/en-us/training/wwl-data-ai/maintain-azure-cognitive-search-solution/media/enriched-document-output-expression-small.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/maintain-azure-cognitive-search-solution/media/enriched-document-output-expression.png#lightbox)
2. In the details pane to the right, select the **Executions** tab, then in OUTPUTS, open the Expression evaluator by selecting **</>** next to **organizations** .
3. To edit the skill, select the **Skill Settings** tab. [![A screenshot showing editing a skill in the Debug Session.](https://learn.microsoft.com/en-us/training/wwl-data-ai/maintain-azure-cognitive-search-solution/media/edit-skill-debug-session-small.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/maintain-azure-cognitive-search-solution/media/edit-skill-debug-session.png#lightbox)
4. Make any changes to the JSON of the skill, then select **Save**.
5. To test that the changes have fixed your issue, select **Run**.
6. If the issue is now resolved and you want to publish the changes, at the top of the pane select **Commit changes...**.
7. To finish the debugging session, select **Save Session**.

### Validate the field mappings

Indexers can be modified if your input data doesn't quite match the schema of your target index. Use field mappings to reshape and fix this mismatch in your data during the indexing process.

1. Select **Skill Graph**, and check that **Dependency graph** is selected. [![A screenshot showing the field mappings pane.](https://learn.microsoft.com/en-us/training/wwl-data-ai/maintain-azure-cognitive-search-solution/media/field-mappings-small.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/maintain-azure-cognitive-search-solution/media/field-mappings.png#lightbox)
2. Select the second step in the enrichment pipeline, **Field Mappings**.
3. Make any changes to where data should be mapped to.
4. Select **Save**.
5. Select the last step, **Output Field Mappings**.
6. Output field mappings from the skills can be fixed in the detail pane.
7. Select **Save**.
8. To test that the changes have fixed your issue, select **Run**.
9. If the issue is now resolved and you want to publish the changes, at the top of the pane select **Commit changes...**.

## Resources

-   Debug search issues Training: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/08-exercise-debug-search-issues.html
