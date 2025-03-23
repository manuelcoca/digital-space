---
tags: azure, azureai, azureopenai, azuregenerativeai
---

# REST API

For each call to the REST API, you need the endpoint and a key from your Azure OpenAI resource, and the name you gave for your deployed model:

| Placeholder name       | Value                                                                                                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `YOUR_ENDPOINT_NAME`   | This base endpoint is found in the **Keys & Endpoint** section in the Azure portal. It's the base endpoint of your resource, such as `https://sample.openai.azure.com/`. |
| `YOUR_API_KEY`         | Keys are found in the **Keys & Endpoint** section in the Azure portal. You can use either key for your resource.                                                         |
| `YOUR_DEPLOYMENT_NAME` | This deployment name is the name provided when you deployed your model in the Azure OpenAI Studio.                                                                       |

## Completions

Make `POST` request to `completions` endpoint to complete prompts:

```
curl https://YOUR_ENDPOINT_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/completions?api-version=2022-12-01\
  -H "Content-Type: application/json" \
  -H "api-key: YOUR_API_KEY" \
  -d "{
  \"prompt\": \"Your favorite Shakespeare is\",
  \"max_tokens\": 5
}"
```

Response:

```
{
    "id": "<id>",
    "object": "text_completion",
    "created": 1679001781,
    "model": "text-davinci-003",
    "choices": [
        {
            "text": "Macbeth",
            "index": 0,
            "logprobs": null,
            "finish_reason": "stop"
        }
    ]
}
```

The completion response to look for is within `choices[].text`. Notice that also included in the response is `finish_reason`, which in this example is `stop`. Other possibilities for `finish_reason` include `length`, which means it used up the `max_tokens` specified in the request, or `content_filter`, which means the system detected harmful content was generated from the prompt. If harmful content is included in the prompt, the API request returns an error.

## Chat completions

Similar to `completions`, `chat/completions` generates a completion to your prompt, but works best when that prompt is a chat exchange:

```
curl https://YOUR_ENDPOINT_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/chat/completions?api-version=2023-03-15-preview \
  -H "Content-Type: application/json" \
  -H "api-key: YOUR_API_KEY" \
  -d '{"messages":[{"role": "system", "content": "You are a helpful assistant, teaching people about AI."},
{"role": "user", "content": "Does Azure OpenAI support multiple languages?"},
{"role": "assistant", "content": "Yes, Azure OpenAI supports several languages, and can translate between them."},
{"role": "user", "content": "Do other Azure AI Services support translation too?"}]}'
```

Response:

```
{
    "id": "chatcmpl-6v7mkQj980V1yBec6ETrKPRqFjNw9",
    "object": "chat.completion",
    "created": 1679001781,
    "model": "gpt-35-turbo",
    "usage": {
        "prompt_tokens": 95,
        "completion_tokens": 84,
        "total_tokens": 179
    },
    "choices": [
        {
            "message":
                {
                    "role": "assistant",
                    "content": "Yes, other Azure AI Services also support translation. Azure AI Services offer translation between multiple languages for text, documents, or custom translation through Azure AI Services Translator."
                },
            "finish_reason": "stop",
            "index": 0
        }
    ]
}
```

Both completion endpoints allow for specifying other optional input parameters, such as `temperature`, `max_tokens` and more. If you'd like to include any of those parameters in your request, add them to the input data with the request.

## Embeddings

Embeddings are helpful for specific formats that are easily consumed by machine learning models. To generate embeddings from the input text, `POST` a request to the `embeddings` endpoint:

```
curl https://YOUR_ENDPOINT_NAME.openai.azure.com/openai/deployments/YOUR_DEPLOYMENT_NAME/embeddings?api-version=2022-12-01 \
  -H "Content-Type: application/json" \
  -H "api-key: YOUR_API_KEY" \
  -d "{\"input\": \"The food was delicious and the waiter...\"}"
```

When generating embeddings, be sure to use a model in AOAI meant for embeddings. Those models start with `text-embedding` or `text-similarity`, depending on what functionality you're looking for.

Response:

```
{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "embedding": [
        0.0172990688066482523,
        -0.0291879814639389515,
        ....
        0.0134544348834753042,
      ],
      "index": 0
    }
  ],
  "model": "text-embedding-ada:002"
}
```

# .NET SDK

## Install libraries

`dotnet add package Azure.AI.OpenAI --prerelease`

## Configure OpenAPI Client

```
// Add OpenAI library
using Azure.AI.OpenAI;

// Define parameters and initialize the client
string endpoint = "<YOUR_ENDPOINT_NAME>";
string key = "<YOUR_API_KEY>";
string deploymentName = "<YOUR_DEPLOYMENT_NAME>"; //SDK calls this "engine", but naming
                                                  // it "deploymentName" for clarity

OpenAIClient client = new OpenAIClient(new Uri(endpoint), new AzureKeyCredential(key));
```

## Call OpenAI Resource

Completion:

```
string prompt = "What is Azure OpenAI?";

Response<Completions> completionsResponse = client.GetCompletions(deploymentName, prompt);
string completion = completionsResponse.Value.Choices[0].Text;
Console.WriteLine($"Chatbot: {completion}");
```

ChatCompletion:

```
// Build completion options object
ChatCompletionsOptions chatCompletionsOptions = new ChatCompletionsOptions()
{
    Messages =
    {
        new ChatMessage(ChatRole.System, "You are a helpful AI bot."),
        new ChatMessage(ChatRole.User, "What is Azure OpenAI?")
    }
};

// Send request to Azure OpenAI model
ChatCompletions chatCompletionsResponse = client.GetChatCompletions(
    deploymentName,
    chatCompletionsOptions);

ChatMessage completion = chatCompletionsResponse.Choices[0].Message;
Console.WriteLine($"Chatbot: {completion.Content}");
```

---

# Resource

Training Resource: https://microsoftlearning.github.io/mslearn-openai/Instructions/Labs/02-natural-language-azure-openai.html
