---
tags: azure, azureai, azureailanguage
---

The endpoint used to query a specific feature varies, but all of them are prefixed with the Azure AI Language resource you created in your Azure account. The endpoint will look something like this:

```
https://{ENDPOINT}/text/analytics/{VERSION}/{FEATURE}
```

| Placeholder  | Value                                                                                                          |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `{ENDPOINT}` | The endpoint for authenticating your API request. For example, `myLanguageService.cognitiveservices.azure.com` |
| `{VERSION}`  | The version number of the service you want to call. For example, `v3.0`                                        |
| `{FEATURE}`  | The feature you're submitting the query to. For example, `keyPhrases` for key phrase detection                 |

# Pre-configured features

## Summarization

A summarization query will be sent to an endpoint similar to the following, with the task specified as `extractiveSummarizationTasks` or `ConversationalSummarizationTask`, depending on which summarization task you want.

```
/{ENDPOINT}/text/analytics/{VERSION}/analyze
```

## Named entity recognition

An entity recognition query will be sent to an endpoint similar to the following, with the task specified as `EntityRecognition`.

```
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
```

## Key phrase extraction

A key phrase extraction query will be sent to an endpoint similar to the following, with the task specified as `KeyPhraseExtraction`.

```
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
```

## Sentiment analysis

A sentiment analysis query will be sent to an endpoint similar to the following, with the task specified as `SentimentAnalysis`.

```
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
```

## Language detection

A language detection query will be sent to an endpoint similar to the following, with the task specified as `LanguageDetection`.

```
/{ENDPOINT}/language/:analyze-text?api-version={VERSION}
```

# Learned features

## Conversational language understanding (CLU)

A language detection query will be sent to an endpoint similar to the following, with the task specified as `Conversation`. These custom features require extra parameters in the JSON body, including the `projectName` and `deploymentName` of your model.

```
/{ENDPOINT}/language/:analyze-conversations?api-version={VERSION}
```

## Custom named entity recognition

A custom entity recognition query will be sent to an endpoint similar to the following, with the task specified as `CustomEntityRecognition`. This custom feature also require extra parameters in the JSON body, including the `projectName` and `deploymentName` of your model.

```
/{ENDPOINT}/language/analyze-text/jobs?api-version={VERSION}
```

## Custom text classification

A custom text classification query will be sent to an endpoint similar to the following, with the task specified as `CustomMultiLabelClassification` or `CustomSingleLabelClassification` depending on if you need single or multi label classification. This custom feature also require extra parameters in the JSON body, including the `projectName` and `deploymentName` of your model.

```
/{ENDPOINT}/language/analyze-text/jobs?api-version={VERSION}
```

## Question answering

For example, say you want to make a virtual chat assistant on your company website to answer common questions. You could use a company FAQ as the input document to create the question and answer pairs. Once deployed, your chat assistant can pass input questions to the service, and get the answers as a result.

The endpoint to query looks similar to the following.

```
{ENDPOINT}/language/:query-knowledgebases?projectName={PROJECTNAME}&deploymentName={DEPLOYMENTNAME}&api-version={VERSION}
```

| Placeholder        | Value                                                                                                          |
| ------------------ | -------------------------------------------------------------------------------------------------------------- |
| `{ENDPOINT}`       | The endpoint for authenticating your API request. For example, `myLanguageService.cognitiveservices.azure.com` |
| `{PROJECTNAME}`    | The name of your project where you provided documents as data to answer questions                              |
| `{DEPLOYMENTNAME}` | The name of your deployment                                                                                    |
| `{VERSION}`        | The api version number of the service you want to call. For example, `2022-05-01`                              |

For a complete list of capabilities and how to use them, see the Azure AI Language [documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/language-service/overview).
