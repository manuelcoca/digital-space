---
tags: azure, azureai, azureailanguage
---

Named Entity Recognition identifies entities that are mentioned in the text. Entities are grouped into categories and subcategories, for example:

-   Person
-   Location
-   DateTime
-   Organization
-   Address
-   Email
-   URL

> For a full list of categories, see the [Azure AI Language documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/language-service/named-entity-recognition/concepts/named-entity-categories).

Input for entity recognition is similar to input for other Azure AI Language API functions:

```
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Joe went to London on Saturday"
    }
  ]
}
```

The response includes a list of categorized entities found in each document:

```
{
  "documents":[
      {
          "id":"1",
          "entities":[
          {
            "text":"Joe",
            "category":"Person",
            "offset":0,
            "length":3,
            "confidenceScore":0.62
          },
          {
            "text":"London",
            "category":"Location",
            "subcategory":"GPE",
            "offset":12,
            "length":6,
            "confidenceScore":0.88
          },
          {
            "text":"Saturday",
            "category":"DateTime",
            "subcategory":"Date",
            "offset":22,
            "length":8,
            "confidenceScore":0.8
          }
        ],
        "warnings":[]
      }
  ],
  "errors":[],
  "modelVersion":"2021-01-15"
}
```

## Linked entities

Azure Language service provides an API to determine the meaning of a word if it can have multiple meanings/entities. Example: Determine if the word "Paris" refers to the French city or the character in Homer's

---

## Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/05-analyze-text.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/05-analyze-text.html)
