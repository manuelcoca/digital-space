---
tags: azure, azureai, azureaisearch
---

Language Studio is included in Azure AI Language Service (see here [[Language Service]]). All Azure Language models can be used to enrich Search Indexes in Cognitive Search:

-   [[Analyse sentiment]]
-   [[Extract key phrases]]
-   [[Named Entity Recognition]]
-   [[Detect language]]
-   [[translator]]
-   Custom [[Text classification]]

Custom text classification can be used to automatically identify book genres based on the book cover. The identified genres can then be used to enrich search index to provide a search based on genres. Steps for this example would be:

1. Store your documents so they can be accessed by Language Studio and Azure Cognitive Search indexers
    1. E.g store data to Azure Blob Storage. Both Cognitive Search and Language Studio can access data from Blob
2. Create a custom text classification project
3. Train and test your model
4. Create a search index based on your stored documents
5. Create a function app that uses your deployed trained model
6. Update your search solution, your index, indexer, and custom skillset

## Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/04-exercise-enrich-cognitive-custom-classes.html
