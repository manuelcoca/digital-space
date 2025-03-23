---
tags: azure, azureai, azureaisearch, azuredatafactory
---

Azure Cognitive Search supports two main ways to get data into a search index:

-   **Pull** data using indexers.
-   **Push** data via SDK, API or external data sources like Azure Data Factory

# Azure Data Factory

Azure Data Factory (ADF) can connect to 100+ data stores. If data is connected with ADF, pipeline can be created to push data into a Cognitive Search index.

The steps you need to take to use and ADF pipeline to push data into a search index are:

1. Create an Azure Cognitive Search index with all the fields you want to store data in.
2. Create a pipeline with a copy data step.
3. Create a data source connection to where your data resides.
4. Create a sink to connect to your search index.
5. Map the fields from your source data to your search index.
6. Run the pipeline to push the data into the index.

For example, imagine you've customer data in JSON format that is hosted externally. You want to copy these customers into a search index. The JSON is in this format:

```
{
  "_id": "5fed1b38309495de1bc4f653",
  "firstName": "Sims",
  "lastName": "Arnold",
  "isAlive": false,
  "age": 35,
  "address": {
    "streetAddress": "Sumner Place",
    "city": "Canoochee",
    "state": "Palau",
    "postalCode": 1558
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "+1 (830) 465-2965"
    },
    {
      "type": "home",
      "number": "+1 (889) 439-3632"
    }
  ]
}
```

## Create a search index

Create an Azure Cognitive Search service and an index to store this information in. If you've completed the [Create an Azure Cognitive Search solution](https://learn.microsoft.com/en-us/training/modules/create-azure-cognitive-search-solution/) module, then you've seen how to do this. Follow the steps to create the search service but stop at the point of importing data. As pushing data into an index doesn't need you to create an indexer or skillset.

Create an index and add these fields and properties:

[![A screenshot of the search index field definitions.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/search-index-fields.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/search-index-fields.png#lightbox)

At the moment you have to create the index first, as ADF can't create indexes.

## Create a pipeline using the ADF Copy Data tool

Open the [Azure Data Factory Studio](https://adf.azure.com/) and select your Azure subscription and data factory name.

[![A screenshot of Azure Data Factory and selecting ingest.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/create-ingestion-pipeline.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/create-ingestion-pipeline.png#lightbox)

1. Select **Ingest**.
2. Select **Next**.

    You can choose to schedule the pipeline if your data is changing and you need to keep your index up-to-date. For this example, you'll import the data once.

## Create the source linked service

1. In **Source type**, select **HTTP**.
2. Next to **Connection**, select **+ New connection**. [![A screenshot showing creating an HTTP linked service.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/create-http-linked-service-new-small.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/create-http-linked-service-new.png#lightbox)
3. In the **New connection** pane, in **Name** enter **dataLocation**.
4. In the **Base URL**, enter where your JSON file resides, in this example enter **[https://raw.githubusercontent.com/Azure-Samples/azure-sql-db-import-data/main/json/user1.json](https://raw.githubusercontent.com/Azure-Samples/azure-sql-db-import-data/main/json/user1.json)**.
5. In **Authentication type**, select **Anonymous**.
6. Select **Create**.
7. Select **Next**. [![A screenshot of the configuration page of the lined service.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/copy-data-configuration.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/copy-data-configuration.png#lightbox)
8. In **File format**, select **JSON**.
9. Select **Next**.

## Create the target linked service

1. In **Destination type**, select **Azure Search**. Then select **+ New connection**. [![A screenshot showing creating a linked service to cognitive search.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/create-search-linked-service-small.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/create-search-linked-service.png#lightbox)
2. In the **New connection** pane, in **Name** enter **search_index**.
3. In **Azure subscription**, select your Azure subscription.
4. In **Service name**, select your Azure Cognitive Search service.
5. Select **Create**.
6. On the **Destination data store** pane, in **Target**, select the search index you created.

#### Map source fields to target fields

1. Select **Next**. [![A screenshot of the schema mapping pane.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/update-schema-mapping-new-small.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/update-schema-mapping-new.png#lightbox)
2. If you created an index with field names that match the JSON attributes ADF will automatically map the JSON to the field in your search index.
3. In the above example, three fields in the JSON document need mapping to fields in the index.
4. Map your fields, then select **Next**.
5. On the **Settings** pane, in **Task name**, enter **jsonToSearchIndex**.
6. Select **Next**.

## Run the pipeline to push the data into the index

1. On the **Summary** pane, select **Next**. [![A screenshot showing the pipeline deployment complete.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/deployment-complete.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/deployment-complete.png#lightbox)
2. Once the pipeline has been validated and deployed, select **Finish**.

The pipeline has been deployed and run. The JSON document will have been added to your search index. You can use the Azure portal and run a search in the search explorer. You should see the imported JSON data.

[![A screenshot of the JSON data in the search index.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/view-search-index-results.png)](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/view-search-index-results.png#lightbox) Following these steps you've seen how you can push data into an index. The pipeline you've created by default merges updates into the index. If you amended the JSON data and rerun the pipeline, the search index would be updated. You can change the write behavior to upload only if you want the data to be replaced each time you run your pipeline.

## Limitations of using the built-in Azure Cognitive Search as a linked service

At the moment, the Azure Cognitive Search linked service as a sink only supports these fields:

| Azure Cognitive Search data type |
| -------------------------------- |
| String                           |
| Int32                            |
| Int64                            |
| Double                           |
| Boolean                          |
| DataTimeOffset                   |

This means ComplexTypes and arrays aren't currently supported. Looking at the JSON document above this means that it isn't possible to map all the phone numbers for the customer. Only the first telephone number has been mapped.

# REST API

The REST API is the most flexible way to push data into an Azure Cognitive Search index. You can use any programming language or interactively with any app that can post JSON requests to an endpoint.

If you want to call any of the search APIs you need to:

-   Use the HTTPS endpoint (over the default port 443) provided by your search service, you must include an **api-version** in the URI.
-   The request header must include an **api-key** attribute.

## Supported REST API operations

| Feature     | Operations                                                     |
| ----------- | -------------------------------------------------------------- |
| Index       | Create, delete, update, and configure.                         |
| Document    | Get, add, update, and delete.                                  |
| Indexer     | Configure data sources and scheduling on limited data sources. |
| Skillset    | Get, create, delete, list, and update.                         |
| Synonym map | Get, create, delete, list, and update.                         |

## Usage example

Endpoint:

```
POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
```

Body:

```
{
  "value": [
    {
      "@search.action": "upload",
      "key_field_name": "unique_key_of_document",
      "field_name": field_value
        ...
    },
    ...
  ]
}
```

-   key_field_name = key/value pair for key field from index schema
-   field_name = key/value pairs matching index schema

Possible search actions:

| Action            | Description                                                                                                 |
| ----------------- | ----------------------------------------------------------------------------------------------------------- |
| **upload**        | Similar to an upsert in SQL, the document will be created or replaced.                                      |
| **merge**         | Merge updates an existing document with the specified fields. Merge will fail if no document can be found.  |
| **mergeOrUpload** | Merge updates an existing document with the specified fields, and uploads it if the document doesn't exist. |
| **delete**        | Deletes the whole document, you only need to specify the key_field_name.                                    |

# .Net SDK

For best performance use the latest `Azure.Search.Document` client library, currently version 11. You can install the client library with NuGet:

```
dotnet add package Azure.Search.Documents --version 11.4.0
```

How your index performs is based on six key factors:

-   The search service tier and how many replicas and partitions you've enabled.
-   The complexity of the index schema. Reduce how many properties (searchable, facetable, sortable) each field has.
-   The number of documents in each batch, the best size will depend on the index schema and the size of documents.
-   How multithreaded your approach is.
-   Handling errors and throttling. Use an exponential backoff retry strategy.
-   Where your data resides, try to index your data as close to your search index. For example, run uploads from inside the Azure environment.

## Work out your optimal batch size

As working out the best batch size is a key factor to improve performance, let's look at an approach in code.

```
public static async Task TestBatchSizesAsync(SearchClient searchClient, int min = 100, int max = 1000, int step = 100, int numTries = 3)
{
    DataGenerator dg = new DataGenerator();

    Console.WriteLine("Batch Size \t Size in MB \t MB / Doc \t Time (ms) \t MB / Second");
    for (int numDocs = min; numDocs <= max; numDocs += step)
    {
        List<TimeSpan> durations = new List<TimeSpan>();
        double sizeInMb = 0.0;
        for (int x = 0; x < numTries; x++)
        {
            List<Hotel> hotels = dg.GetHotels(numDocs, "large");

            DateTime startTime = DateTime.Now;
            await UploadDocumentsAsync(searchClient, hotels).ConfigureAwait(false);
            DateTime endTime = DateTime.Now;
            durations.Add(endTime - startTime);

            sizeInMb = EstimateObjectSize(hotels);
        }

        var avgDuration = durations.Average(timeSpan => timeSpan.TotalMilliseconds);
        var avgDurationInSeconds = avgDuration / 1000;
        var mbPerSecond = sizeInMb / avgDurationInSeconds;

        Console.WriteLine("{0} \t\t {1} \t\t {2} \t\t {3} \t {4}", numDocs, Math.Round(sizeInMb, 3), Math.Round(sizeInMb / numDocs, 3), Math.Round(avgDuration, 3), Math.Round(mbPerSecond, 3));

        // Pausing 2 seconds to let the search service catch its breath
        Thread.Sleep(2000);
    }

    Console.WriteLine();
}
```

The approach is to increase the batch size and monitor the time it takes to receive a valid response. The code loops from 100 to 1000, in 100 document steps. For each batch size, it outputs the document size, time to get a response, and the average time per MB. Running this code gives results like this:

![A screenshot of the output from the code above.](https://learn.microsoft.com/en-us/training/wwl-data-ai/search-data-outside-azure-platform-cognitive-search/media/batch-program-output.png)

In the above example, the best batch size for throughput is **2.499** MB per second, **800** documents per batch.

## Implement an exponential backoff retry strategy

If your index starts to throttle requests due to overloads, it responds with a 503 (request rejected due to heavy load) or 207 (some documents failed in the batch) status. You have to handle these responses and a good strategy is to backoff. Backing off means pausing for some time before retrying your request again. If you increase this time for each error, you'll be exponentially backing off.

Look at this code:

```
// Implement exponential backoff
do
{
    try
    {
        attempts++;
        result = await searchClient.IndexDocumentsAsync(batch).ConfigureAwait(false);

        var failedDocuments = result.Results.Where(r => r.Succeeded != true).ToList();

        // handle partial failure
        if (failedDocuments.Count > 0)
        {
            if (attempts == maxRetryAttempts)
            {
                Console.WriteLine("[MAX RETRIES HIT] - Giving up on the batch starting at {0}", id);
                break;
            }
            else
            {
                Console.WriteLine("[Batch starting at doc {0} had partial failure]", id);
                Console.WriteLine("[Retrying {0} failed documents] \n", failedDocuments.Count);

                // creating a batch of failed documents to retry
                var failedDocumentKeys = failedDocuments.Select(doc => doc.Key).ToList();
                hotels = hotels.Where(h => failedDocumentKeys.Contains(h.HotelId)).ToList();
                batch = IndexDocumentsBatch.Upload(hotels);

                Task.Delay(delay).Wait();
                delay = delay * 2;
                continue;
            }
        }

        return result;
    }
    catch (RequestFailedException ex)
    {
        Console.WriteLine("[Batch starting at doc {0} failed]", id);
        Console.WriteLine("[Retrying entire batch] \n");

        if (attempts == maxRetryAttempts)
        {
            Console.WriteLine("[MAX RETRIES HIT] - Giving up on the batch starting at {0}", id);
            break;
        }

        Task.Delay(delay).Wait();
        delay = delay * 2;
    }
} while (true);
```

The code keeps track of failed documents in a batch. If an error happens, it waits for a delay and then doubles the delay for the next error.

Finally, there's a maximum number of retries, and if this maximum number is reached the program exists.

## Use threading to improve performance

You can complete your document uploading app by combing the above backoff strategy with a threading approach. Here's some example code:

```
        public static async Task IndexDataAsync(SearchClient searchClient, List<Hotel> hotels, int batchSize, int numThreads)
        {
            int numDocs = hotels.Count;
            Console.WriteLine("Uploading {0} documents...\n", numDocs.ToString());

            DateTime startTime = DateTime.Now;
            Console.WriteLine("Started at: {0} \n", startTime);
            Console.WriteLine("Creating {0} threads...\n", numThreads);

            // Creating a list to hold active tasks
            List<Task<IndexDocumentsResult>> uploadTasks = new List<Task<IndexDocumentsResult>>();

            for (int i = 0; i < numDocs; i += batchSize)
            {
                List<Hotel> hotelBatch = hotels.GetRange(i, batchSize);
                var task = ExponentialBackoffAsync(searchClient, hotelBatch, i);
                uploadTasks.Add(task);
                Console.WriteLine("Sending a batch of {0} docs starting with doc {1}...\n", batchSize, i);

                // Checking if we've hit the specified number of threads
                if (uploadTasks.Count >= numThreads)
                {
                    Task<IndexDocumentsResult> firstTaskFinished = await Task.WhenAny(uploadTasks);
                    Console.WriteLine("Finished a thread, kicking off another...");
                    uploadTasks.Remove(firstTaskFinished);
                }
            }

            // waiting for the remaining results to finish
            await Task.WhenAll(uploadTasks);

            DateTime endTime = DateTime.Now;

            TimeSpan runningTime = endTime - startTime;
            Console.WriteLine("\nEnded at: {0} \n", endTime);
            Console.WriteLine("Upload time total: {0}", runningTime);

            double timePerBatch = Math.Round(runningTime.TotalMilliseconds / (numDocs / batchSize), 4);
            Console.WriteLine("Upload time per batch: {0} ms", timePerBatch);

            double timePerDoc = Math.Round(runningTime.TotalMilliseconds / numDocs, 4);
            Console.WriteLine("Upload time per document: {0} ms \n", timePerDoc);
        }
```

This code uses asynchronous calls to a function `ExponentialBackoffAsync` that implements the backoff strategy. You call the function using threads, for example, the number of cores your processor has. When the maximum number of threads has been used, the code waits for any thread to finish. It then creates a new thread until all the documents are uploaded.

# Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/07-exercise-add-to-index-use-push-api.html
