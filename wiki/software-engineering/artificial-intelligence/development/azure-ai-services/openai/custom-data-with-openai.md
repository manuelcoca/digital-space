---
tags: azure, azureai, azureopenai, azuregenerativeai
---

## How OpenAI works with custom data

Azure OpenAI can generate responses based on both custom data and its pre-trained model to provide more effective responses

Azure OpenAI on your data utilizes the search ability of Azure Cognitive Search to add the relevant data chunks to the prompt. Once your data is in a Cognitive Search index, Azure OpenAI on your data goes through the following steps:

1. Receive user prompt.
2. Determine relevant content and intent of the prompt.
3. Query the search index with that content and intent.
4. Insert search result chunk into the Azure OpenAI prompt, along with system message and user prompt.
5. Send entire prompt to Azure OpenAI.
6. Return response and data reference (if any) to the user.

By default, Azure OpenAI on your data encourages, but doesn't require, the model to respond only using your data. This setting can be unselected when connecting your data, which may result in the model choosing to use its pretrained knowledge over your data.

## Fine-tuning vs. using your own data

Fine-tuning is a method for customizing a foundational model like 'gpt-3.5-turbo' by training it with additional data. It can improve response quality, handle larger examples, and require fewer examples for high-quality results. However, fine-tuning is costly and time-consuming, so it should be reserved for necessary use cases.

Azure OpenAI on your data simplifies interaction with AI models by using the stateless API, eliminating the need for custom model training. It leverages Cognitive Search to extract relevant information and form responses based on that data.

## Add custom data

Adding custom data is done through the Azure AI Studio, in the **Chat** playground. Cou can choose to upload your data files, use data in a blob storage account, or connect to an existing Cognitive Search index.

If you're uploading or using files already in a storage account, Azure OpenAI on your data supports `.md`, `.txt`, `.html`, `.pdf`, and Microsoft Word or PowerPoint files. If any of these files contain graphics or images, the response quality depends on how well text can be extracted from the visual content.

When uploading data or connecting to files in a storage account, it's recommended to use the Azure AI Studio to create the search resource and index. Adding data this way allows the appropriate chunking to happen when inserting into the index, yielding better responses. If you're using large text files or forms, you should use the available [data preparation script](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/concepts/use-your-data#ingesting-your-data-into-azure-cognitive-search?azure-portal=true) to improve the AI model's accuracy.

Enabling [semantic search](https://learn.microsoft.com/en-us/azure/search/semantic-search-overview) for your Cognitive Search service can improve the result of searching your data index and you're likely to receive higher quality responses and citations. However, enabling semantic search may increase the cost of the search service.

## Connect data

-   Open Chat Playground in Azure AI Studio
-   Select **Add your data** tab in the **Assistan setup** pane
-   Click **Add a data source**
-   The prompts guide you through setting up the connection to each data source, and getting that data into a search index.

## Use custom data

The call when using your own data needs to be sent to a different REST endpoint than is used when calling a base model, which includes `extensions`: `<your_azure_openai_resource>/openai/deployments/<deployment_name>/extensions/chat/completions?api-version=2023-06-01-preview`

The request need to include the `Content-Type`, `api-key` and `endpoint`, `key`, and `indexName` for your Cognitive Search resource, where custom data is stored.

Your request body will be similar to the following JSON.

```
{
    "dataSources": [
        {
            "type": "AzureCognitiveSearch",
            "parameters": {
                "endpoint": "<your_search_endpoint>",
                "key": "<your_search_endpoint>",
                "indexName": "<your_search_index>"
            }
        }
    ],
    "messages":[
        {
            "role": "system",
            "content": "You are a helpful assistant assisting users with                          travel recommendations."
        },
        {
            "role": "user",
            "content": "I want to go to New York. Where should I stay?"
        }
    ]
}
```

### Token limit considerations

-   System message gets truncated if it exceeds 200 tokens when using custom data.
-   Response from model is limited to 1500 tokens when using custom data.

## Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-openai/Instructions/Labs/06-use-own-data.html
