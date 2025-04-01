---
tags: azure, azureai, azureopenai, azuregenerativeai
---

## Create an Azure OpenAI Service Resource

Azure OpenAI Service can be created in the portal or via CLI:

-   MyOpenAIResource: *replace with a unique name for your resource*
-   OAIResourceGroup: *replace with your resource group name*
-   eastus: *replace with the region to deploy your resource*
-   subscriptionID: *replace with your subscription ID*

```
az cognitiveservices account create \
-n MyOpenAIResource \
-g OAIResourceGroup \
-l eastus \
--kind OpenAI \
--sku s0 \
--subscription subscriptionID
```

## Azure OpenAI Studio

After resource is created we can login to Azure OpenAI Studio: https://oai.azure.com/ During the sign-in workflow, select the appropriate directory, Azure subscription, and Azure OpenAI resource.

Azure OpenAI Studio can be used to deploy and test models.

## Generative AI Models

Microsoft provides base models and the option to create customized base models. Currently available base models:

-   **GPT-4 models** are the latest generation of *generative pretrained* (GPT) models that can generate natural language and code completions based on natural language prompts.
-   **GPT 3.5 models** can generate natural language and code completions based on natural language prompts. In particular, **GPT-35-turbo** models are optimized for chat-based interactions and work well in most generative AI scenarios.
-   **Embeddings models** convert text into numeric vectors, and are useful in language analytics scenarios such as comparing text sources for similarities.
-   **DALL-E models** are used to generate images based on natural language prompts. Currently, DALL-E models are in preview. DALL-E models aren't listed in the Azure OpenAI Studio interface and don't need to be explicitly deployed.

Models differ by speed, cost, and how well they complete specific tasks. You can learn more about the differences and latest models offered in the [Azure OpenAI Service documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/concepts/models).

> Certain models are only available in select regions. Consult the [Azure OpenAI model availability guide](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/concepts/models#model-summary-table-and-region-availability/?azure-portal=true) for region availability. You can create two Azure OpenAI resources per region.

In the Azure OpenAI Studio, the **Models** page lists the available base models (other than DALL-E models) and provides an option to create additional customized models by fine-tuning the base models. The models that have a *Succeeded* status mean they're successfully trained and can be selected for deployment.

![[studio-models.png]]

## Deploy a Model

Use Azure OpenAI Studio, CLI or REST API to deploy a model and begin building with Azure OpenAI.

### Deploy using Azure OpenAI Studio

![[openai-portal-navigation.png]]![[studio-deployment.png]]

### Deploy using Azure CLI

-   myResourceGroupName: *replace with your resource group name*
-   myResourceName: *replace with your resource name*
-   MyModel: *replace with a unique name for your model*
-   gpt-35-turbo: *replace with the base model you wish to deploy*

```
az cognitiveservices account deployment create \
   -g myResourceGroupName \
   -n myResourceName \
   --deployment-name MyModel \
   --model-name gpt-35-turbo \
   --model-version "0301"  \
   --model-format OpenAI \
   --scale-settings-scale-type "Standard"
```

### Deploy using the REST API

You can deploy a model using the REST API. In the request body, you specify the base model you wish to deploy. See an example in the [Azure OpenAI documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/).

## Test a Model

Once the model is deployed, you can test how it completes prompts via Azure OpenAI Studio, REST API or SDK. A prompt is the text portion of a request that is sent to the deployed model's completions endpoint. Responses are referred to as *completions*, which can come in form of text, code, or other formats.

### Prompt types

Prompts can be grouped into types of requests based on task.

| Task type                                              | Prompt example                               | Completion example                                                                                                            |
| ------------------------------------------------------ | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Classifying content**                                | Tweet: I enjoyed the trip.  <br>Sentiment:   | Positive                                                                                                                      |
| **Generating new content**                             | List ways of traveling                       | 1. Bike  <br>2. Car ...                                                                                                       |
| **Holding a conversation**                             | A friendly AI assistant                      | [See examples](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/how-to/completions#conversation?portal=true) |
| **Transformation** (translation and symbol conversion) | English: Hello  <br>French:                  | bonjour                                                                                                                       |
| **Summarizing content**                                | Provide a summary of the content  <br>{text} | The content shares methods of machine learning.                                                                               |
| **Picking up where you left off**                      | One way to grow tomatoes                     | is to plant seeds.                                                                                                            |
| **Giving factual responses**                           | How many moons does Earth have?              | One                                                                                                                           |

### Prompt Engineering

Response quality from large language models (LLMs) in Azure OpenAI depends on the quality of the prompt provided. This makes prompt construction an important skill to develop. Improving prompt quality through various techniques is called prompt engineering. Read more on [[Prompt Engineering]]

### Completions playground

Use the Completions Playground in Azure OpenAI Studio to test the model with prompts.

![[azure-openai-completions-playground.png]]

There are many parameters that you can adjust to improve the performance of your model in the completions playground (read here [[Prompt Engineering]]).

### Chat Playground

Use the Chat Playground In order to test conversational prompts.

![[azure-openai-chat-playground.png]]

The Chat playground, like the Completions playground, also includes the Temperature parameter. The Chat playground also supports other parameters *not* available in the Completions playground (read here [[Prompt Engineering]]).

## Use Deployed Models in Applications

AOAI can be accessed via a REST API or an SDK currently available for Python and C# and generally provides 3 endpoints:

-   **Completion** - model takes an input prompt, and generates one or more predicted completions
-   **ChatCompletion** - model takes input in the form of a chat conversation (where roles are specified with the message they send), and the next chat completion is generated
-   **Embeddings** - model takes input and returns a vector representation of that input

See here how the models can be implemented in Applications:

-   [[Natural Language Solutions]]
    -   Completion
    -   Chat
    -   Embeddings
-   [[Generate Code]]

## Resources

-   Training Resources: https://microsoftlearning.github.io/mslearn-openai/Instructions/Labs/01-get-started-azure-openai.html
