---
tags: azure, azureai, azureailanguage
---

# Language Detection

[Microsoft's Language Detection API](microsofttranslator) can be used to detect the language first, in order to translate text in a second step.

The REST API endpoint is not open. Service region and Subscription Key need to be provided:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/detect?api-version=3.0" -H "Ocp-Apim-Subscription-Region: <your-service-region>" -H "Ocp-Apim-Subscription-Key: <your-key>" -H "Content-Type: application/json" -d "[{ 'Text' : 'こんにちは' }]
```

The above example CURL Request will return following response detecting Japan ("ja") as language:

```
[
  {
    "language": "ja",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": true
   }
]
```

# Translation

The same [API](version) can be used with a different endpoint for translation

To translate text, just pass to parameter **from** and to. CURL request example to translate from Japanese to French and English (**&from=ja&to=fr&to=en**):

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=ja&to=fr&to=en" -H "Ocp-Apim-Subscription-Key: <your-key>" -H "Ocp-Apim-Subscription-Region: <your-service-region>" -H "Content-Type: application/json; charset=UTF-8" -d "[{ 'Text' : 'こんにちは' }]"
```

Response:

```
[
  {"translations":
    [
      {"text": "Hello", "to": "en"},
      {"text": "Bonjour", "to": "fr"}
    ]
  }
]
```

# Transliteration

The Japanese text is written using Hiragana script, so rather than translate it to a different language, you may want to transliterate it to a different script - for example to render the text in Latin script (as used by English language text).

To accomplish this, we can submit the Japanese text to the **Transliterate** function with a **fromScript** parameter of **Jpan**and a **toScript** parameter of **Latn**:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&fromScript=Jpan&toScript=Latn" -H "Ocp-Apim-Subscription-Key: <your-key>" -H "Ocp-Apim-Subscription-Region: <your-service-region>" -H "Content-Type: application/json" -d "[{ 'Text' : 'こんにちは' }]"
```

The response would give you the following result:

```
[
    {
        "script": "Latn",
        "text": "Kon'nichiwa"
    }
]
```

# Translation options

## Sentence length

Sometimes it might be useful to know the length of a translation, for example to determine how best to display it in a user interface. You can get this information by setting the **includeSentenceLength** parameter to **true**.

For example, specifying this parameter when translating the English (**en**) text "Hello world" to French (**fr**) produces the following results:

```
[
   {
      "translations":[
         {
            "text":"Salut tout le monde",
            "to":"fr",
            "sentLen":{"srcSentLen":[12],"transSentLen":[20]}
         }
      ]
   }
]
```

## Profanity filtering

Sometimes text contains profanities, which you might want to obscure or omit altogether in a translation. You can handle profanities by specifying the **profanityAction** parameter, which can have one of the following values:

-   **NoAction**: Profanities are translated along with the rest of the text.
-   **Deleted**: Profanities are omitted in the translation.
-   **Marked**: Profanities are indicated using the technique indicated in the **profanityMarker** parameter (if supplied). The default value for this parameter is **Asterisk**, which replaces characters in profanities with "\*". As an alternative, you can specify a **profanityMarker** value of **Tag**, which causes profanities to be enclosed in XML tags.

For example, translating the English (**en**) text "JSON is ▇▇▇▇ great!" (where the blocked out word is a profanity) to German (**de**) with a **profanityAction** of **Marked** and a **profanityMarker** of **Asterisk** produces the following result:

```
[
   {
      "translations":[
         {
            "text":"JSON ist *** erstaunlich.",
            "to":"de"
         }
      ]
   }
]
```

# Custom Translations

While the default translation model used by Azure AI Translator is effective for general translation, you may need to develop a translation solution for businesses or industries in that have specific vocabularies of terms that require custom translation.

To solve this problem, you can create a custom model that maps your own sets of source and target terms for translation. To create a custom model, use the Custom Translator portal to:

1. [Create a workspace](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/quickstart) linked to your Azure AI Translator resource.
2. [Create a project](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/quickstart).
3. [Upload training data files](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/quickstart) and [train a model](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/quickstart).
4. [Test your model](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/quickstart) and [publish your model](https://learn.microsoft.com/en-us/azure/ai-services/translator/custom-translator/quickstart).
5. Make translation calls to the API.
