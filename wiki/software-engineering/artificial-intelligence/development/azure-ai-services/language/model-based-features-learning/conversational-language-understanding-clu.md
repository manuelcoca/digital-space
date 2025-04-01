---
tags: azure, azureai, azureailanguage
---

*Natural language understanding* (NLU) is a common AI problem in which software must be able to work with text or speech in the natural language form that a human user would write or speak. It deals with the problem of determining semantic meaning from natural language - usually by using a trained language model.

**Azure AI Language** **CLU feature** enables developers to build apps based on language models that can be trained with a relatively small number of samples to discern a user's intended meaning.

## Understand intents, utterances and entities

*Utterances* are the phrases that a user could enter when interacting with an application that uses your language model. An *intent* represents a task or action the user wants to perform, or more simply the *meaning* of an utterance. You create a model by defining intents and associating them with one or more utterances. Further, entities provide specific context to intents, e.g., defining devices for actions like "TurnOnDevice."

For example, consider the following list of intents and associated utterances and entities:

| Utterance                                   | Intent       | Entities                            |
| ------------------------------------------- | ------------ | ----------------------------------- |
| What is the time?                           | GetTime      |                                     |
| What time is it in *London*?                | GetTime      | Location (London)                   |
| Tell me the time                            | GetTime      |                                     |
| What's the weather forecast for *Paris*?    | GetWeather   | Location (Paris)                    |
| Will I need an umbrella *tonight*?          | GetWeather   | Time (tonight)                      |
| What's the forecast for *Seattle tomorrow*? | GetWeather   | Location (Seattle), Time (tomorrow) |
| Turn the *light* on.                        | TurnOnDevice | Device (light)                      |
| Switch on the *fan*.                        | TurnOnDevice | Device (fan)                        |
| Hello                                       | None         |                                     |
| Goodbye                                     | None         |                                     |

## Model training considerations

To train a model define the intents your model should understand by considering the relevant domain and user actions/information. Consider following:

-   Include a "None" intent for miscellaneous or out-of-scope user inputs
-   Collect various example utterances for each intent
-   Capture alternative ways of saying the same thing
-   Vary utterance length and noun/subject location
-   Use correct and incorrect grammar for diverse training data
-   Precise, consistent, and complete labeling is crucial for model performance
-   Label entities to the correct type, maintain consistency, and label all instances

You can split entities into a few different component types:

-   **Learned** entities are the most flexible kind of entity, and should be used in most cases. You define a learned component with a suitable name, and then associate words or phrases with it in training utterances. When you train your model, it learns to match the appropriate elements in the utterances with the entity.
-   **List** entities are useful when you need an entity with a specific set of possible values - for example, days of the week. You can include synonyms in a list entity definition, so you could define a **DayOfWeek** entity that includes the values "Sunday", "Monday", "Tuesday", and so on; each with synonyms like "Sun", "Mon", "Tue", and so on.
-   **Prebuilt** entities are useful for common types such as numbers, datetimes, emails and names. For example, when prebuilt components are added, you will automatically detect values such as "6" or organizations such as "Microsoft". You can see this article for a list of [supported prebuilt entities](https://learn.microsoft.com/en-us/azure/ai-services/language-service/conversational-language-understanding/prebuilt-component-reference).

## Query model

After your model is trained and deployed, you can start using it to make predictions through the [prediction API](https://aka.ms/clu-apis).

Create a **POST** request using the following URL, headers, and JSON body to start testing a conversational language understanding model.

### Request URL

```
{ENDPOINT}/language/:analyze-conversations?api-version={API-VERSION}
```

| Placeholder     | Value                                                                                                                                                 | Example                                                       |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `{ENDPOINT}`    | The endpoint for authenticating your API request.                                                                                                     | `https://<your-custom-subdomain>.cognitiveservices.azure.com` |
| `{API-VERSION}` | The [version](https://learn.microsoft.com/en-us/azure/ai-services/language-service/concepts/model-lifecycle#api-versions) of the API you are calling. | `2023-04-01`                                                  |

### Headers

Use the following header to authenticate your request.

| Key                         | Value                                                                |
| --------------------------- | -------------------------------------------------------------------- |
| `Ocp-Apim-Subscription-Key` | The key to your resource. Used for authenticating your API requests. |

### Request body

```
{
  "kind": "Conversation",
  "analysisInput": {
    "conversationItem": {
      "id": "1",
      "participantId": "1",
      "text": "Text 1"
    }
  },
  "parameters": {
    "projectName": "{PROJECT-NAME}",
    "deploymentName": "{DEPLOYMENT-NAME}",
    "stringIndexType": "TextElement_V8"
  }
}
```

| Key              | Placeholder         | Value                                                                        | Example              |
| ---------------- | ------------------- | ---------------------------------------------------------------------------- | -------------------- |
| `participantId`  | `{JOB-NAME}`        |                                                                              | `"MyJobName`         |
| `id`             | `{JOB-NAME}`        |                                                                              | `"MyJobName`         |
| `text`           | `{TEST-UTTERANCE}`  | The utterance that you want to predict its intent and extract entities from. | `"Read Matt's email` |
| `projectName`    | `{PROJECT-NAME}`    | The name of your project. This value is case-sensitive.                      | `myProject`          |
| `deploymentName` | `{DEPLOYMENT-NAME}` | The name of your deployment. This value is case-sensitive.                   | `staging`            |

Once you send the request, you will get the following response for the prediction.

### Response body

```
{
  "kind": "ConversationResult",
  "result": {
    "query": "Text1",
    "prediction": {
      "topIntent": "inten1",
      "projectKind": "Conversation",
      "intents": [
        {
          "category": "intent1",
          "confidenceScore": 1
        },
        {
          "category": "intent2",
          "confidenceScore": 0
        },
        {
          "category": "intent3",
          "confidenceScore": 0
        }
      ],
      "entities": [
        {
          "category": "entity1",
          "text": "text1",
          "offset": 29,
          "length": 12,
          "confidenceScore": 1
        }
      ]
    }
  }
}
```

| Key       | Sample Value        | Description                                                                                            |
| --------- | ------------------- | ------------------------------------------------------------------------------------------------------ |
| query     | "Read Matt's email" | the text you submitted for query.                                                                      |
| topIntent | "Read"              | The predicted intent with highest confidence score.                                                    |
| intents   | []                  | A list of all the intents that were predicted for the query text each of them with a confidence score. |
| entities  | []                  | array containing list of extracted entities from the query text.                                       |

## API response for a conversation project

In a conversations project, you'll get predictions for both your intents and entities that are present within your project.

-   The intents and entities include a confidence score between 0.0 to 1.0 associated with how confident the model is about predicting a certain element in your project.
-   The top scoring intent is contained within its own parameter.
-   Only predicted entities will show up in your response.
-   Entities indicate:
    -   The text of the entity that was extracted
    -   Its start location denoted by an offset value
    -   The length of the entity text denoted by a length value.

---

## Resources

-   Training Resource: [https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Labs/03-language-understanding.html](https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Labs/03-language-understanding.html)
