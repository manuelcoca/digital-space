---
tags: azure, azureai, azureailanguage
---

Custom NER enables developers to extract pre-defined entities from text documents, without those documents being in a known format. These entities could be anything from names and addresses from bank statements to knowledge mining to improve search results.

For this a custom model need to be trained:

1. Create a project and define entities
2. Label data
3. Train model
4. View model
5. Improve model
6. Deploy model
7. Query API

# Project limits

The Azure AI Language Service enforces the following restrictions:

-   **Training** - at least 10 files, and not more than 100,000
-   **Deployments** - 10 deployment names per project
-   **APIs**
    -   **Authoring** - this API creates a project, trains and deploy your model. Limited to 10 POST and 100 GET per minute
    -   Analyse - this API does the work of actually extracting the entities; it requests a task and retrieves the results. Limited to 20 GET or POST
-   **Projects** - only 1 storage account per project, 500 projects per resource, and 50 trained models per project
-   **Entities** - each entity must be fewer than 10 words and 100 characters, up to 200 entity types, and at least 10 tagged instances per entity

# Label data

Labeling, or tagging, your data correctly is an important part of the process to create a custom entity extraction model. Labels identify examples of specific entities in text used to train the model. Three things to focus on are:

-   **Consistency** - Label your data the same way across all files for training. Consistency allows your model to learn without any conflicting inputs.
-   **Precision** - Label your entities consistently, without unnecessary extra words. Precision ensures only the correct data is included in your extracted entity.
-   **Completeness** - Label your data completely, and don't miss any entities. Completeness helps your model always recognize the entities present.

Data can be labeled via API or Azure AI Language Studio: ![[ner-tag-entity-screenshot.png]]

# Query API

Example Payload for the request:

```
{
    "displayName": "string",
    "analysisInput": {
        "documents": [
            {
                "id": "doc1",
                "text": "string"
            },
            {
                "id": "doc2",
                "text": "string"
            }
        ]
    },
    "tasks": {
        "customEntityRecognitionTasks": [
            {
                "parameters": {
                      "project-name": "myProject",
                      "deployment-name": "myDeployment"
                }
            }
        ]
    }
}
```

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Labs/05-extract-custom-entities.html](https://microsoftlearning.github.io/mslearn-ai-language/Instructions/Labs/05-extract-custom-entities.html)
