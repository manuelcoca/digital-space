---
tags:
    - azure
    - azureai
    - azureailanguage
---

Sentiment analysis is used to evaluate how positive or negative a text document is, which can be useful in various workloads, such as:

-   Evaluating a movie, book, or product by quantifying sentiment based on reviews.
-   Prioritising customer service responses to correspondence received through email or social media messaging.

When using the Azure AI Language service to evaluate sentiment, the response includes overall document sentiment and individual sentence sentiment for each document submitted to the service.

or example, you could submit a single document for sentiment analysis like this:

```
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Smile! Life is good!"
    }
  ]
}
```

The response from the service might look like this:

```
{
  "documents": [
   {
     "id": "1",
     "sentiment": "positive",
     "confidenceScores": {
       "positive": 0.99,
       "neutral": 0.01,
       "negative": 0.00
     },
     "sentences": [
       {
         "text": "Smile!",
         "sentiment": "positive",
         "confidenceScores": {
             "positive": 0.97,
	         "neutral": 0.02,
             "negative": 0.01
           },
         "offset": 0,
         "length": 6
       },
       {
	      "text": "Life is good!",
          "sentiment": "positive",
          "confidenceScores": {
             "positive": 0.98,
	         "neutral": 0.02,
             "negative": 0.00
           },
         "offset": 7,
         "length": 13
       }
     ],
     "warnings": []
   }
  ],
  "errors": [],
  "modelVersion": "2020-04-01"
}
```

Sentence sentiment is based on confidence scores for **positive**, **negative**, and **neutral** classification values between 0 and 1.

Overall document sentiment is based on sentences:

-   If all sentences are neutral, the overall sentiment is neutral.
-   If sentence classifications include one positive and neutral, the overall sentiment is positive.
-   If the sentence classifications include one negative and neutral, the overall sentiment is negative.
-   If the sentence classifications include positive and negative, the overall sentiment is mixed.

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/05-analyze-text.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/05-analyze-text.html)
