---
tags: azure, azureai, azureailanguage
---

# Detect language

-   Azure AI Language Detection API identifies language in text input
-   Returns language identifiers with a confidence score (0 to 1)
    -   1 = Very confident
-   Recognises up to 120 languages.
-   Use-cases:
    -   Useful for content stores with unknown language text.
    -   Helps chatbots determine user language for tailored responses.
-   Works for documents or single phrases, but documents must be < 5,120 characters.
-   Collection limit is 1,000 items.

Example Request JSON Payload:

```
{
  "documents": [
    {
      "countryHint": "US",
      "id": "1",
      "text": "Hello world"
    },
    {
      "id": "2",
      "text": "Bonjour tout le monde"
    }
  ]
}
```

Example Response JSON for above Request:

```
{
  "documents": [
   {
     "id": "1",
     "detectedLanguage": {
       "name": "English",
       "iso6391Name": "en",
       "confidenceScore": 1
     },
     "warnings": []
   },
   {
     "id": "2",
     "detectedLanguage": {
       "name": "French",
       "iso6391Name": "fr",
       "confidenceScore": 1
     },
     "warnings": []
   }
  ],
  "errors": [],
  "modelVersion": "2020-04-01"
}
```

# Request with multiple languages

If you pass in a document that has multilingual content, the service will return the language with the largest representation in the content.

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

The following sample shows a response for this multi-language example.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/05-analyze-text.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/05-analyze-text.html)
