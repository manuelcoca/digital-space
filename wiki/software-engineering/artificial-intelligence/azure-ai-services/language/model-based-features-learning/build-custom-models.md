---
tags: azure, azureai, azureailanguage
---

Building a custom model always follows the same pattern. By following this iterative approach, you can improve the language model over time based on user input, helping you develop solutions that reflect the way users indicate their intents using natural language:

1. Create a project
2. Import data, label data, create intent, entities etc.
    1. It depends on the feature:
        1. CLU - Create intents and entities from sample utterances
        2. NER - Label data
3. Train model
4. View model - View result of trained model
5. Improve model - Improve model based on train results
6. Deploy model - Once your model performs as desired, deploy your model to make it available via the API
7. Query API and review predictions and improve model further

A custom model can be build either via [**REST API**](/o/dG9lU0KHIdHcxa94EY4e/s/P6fNNGB9iCrfE7Zsn3eW/azure-ai-services/language-service/model-based-features-learning/build-custom-models#via-rest-api) or [**Azure** **Language Studio**](/o/dG9lU0KHIdHcxa94EY4e/s/P6fNNGB9iCrfE7Zsn3eW/azure-ai-services/language-service/model-based-features-learning/build-custom-models#via-language-studio).

-   Read detailed [REST API documentation](https://learn.microsoft.com/en-us/azure/ai-services/language-service/conversational-language-understanding/quickstart?pivots=rest-api#create-a-clu-project)
-   Read detailed [Azure Language Studio Documentation](https://learn.microsoft.com/en-us/azure/ai-services/language-service/language-studio)

# Train and evaluate custom models

Training and evaluating your model is an iterative process of adding data and labels to your training dataset. To know what types of data and labels need to be improved, Azure AI Language Studio provides scoring in the **View model details** page on the left hand pane.

![[model-scoring.png]]

Individual entities and your overall model score are broken down into three metrics to explain how they're performing and where they need to improve.

| Metric    | Description                                                                                                                                                                                             |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Precision | The ratio of successful entity recognitions to all attempted recognitions. A high score means that as long as the entity is recognized, it's labeled correctly.                                         |
| Recall    | The ratio of successful entity recognitions to the actual number of entities in the document. A high score means it finds the entity or entities well, regardless of if it assigns them the right label |
| F1 score  | Combination of precision and recall providing a single scoring metric                                                                                                                                   |

Scores are available both per entity and for the model as a whole. You may find an entity scores well, but the whole model doesn't.

# How to interpret metrics

Ideally we want our model to score well in both precision and recall, which means the entity recognition works well. If both metrics have a low score, it means the model is both struggling to recognize entities in the document, and when it does extract that entity, it doesn't assign it the correct label with high confidence.

If precision is low but recall is high, it means that the model recognizes the entity well but doesn't label it as the correct entity type.

If precision is high but recall is low, it means that the model doesn't always recognize the entity, but when the model extracts the entity, the correct label is applied.

# Confusion matric

On the same **View model details** page, there's another tab on the top for the *Confusion matrix*. This view provides a visual table of all the entities and how each performed, giving a complete view of the model and where it's falling short.

![](https://learn.microsoft.com/en-us/training/wwl-data-ai/custom-name-entity-recognition/media/model-confusion-matrix.png#lightbox)

Screenshot of a sample confusion matrix.

The confusion matrix allows you to visually identify where to add data to improve your model's performance.
