---
tags: azure, azureai, azureailanguage
---

# Question Answering vs. CLU

The two features are similar in that they both enable you to define a language model that can be queried using natural language expressions. However, there are some differences in the use cases that they are designed to address, as shown in the following table:

| Text             | Question answering                                                                                   | Language understanding                                                                                               |
| ---------------- | ---------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Usage pattern    | User submits a question, expecting an answer                                                         | User submits an utterance, expecting an appropriate response or action                                               |
| Query processing | Service uses natural language understanding to match the question to an answer in the knowledge base | Service uses natural language understanding to interpret the utterance, match it to an intent, and identify entities |
| Response         | Response is a static answer to a known question                                                      | Response indicates the most likely intent and referenced entities                                                    |
| Client logic     | Client application typically presents the answer to the user                                         | Client application is responsible for performing appropriate action based on the detected intent                     |

The two services are in fact complementary. You can build comprehensive natural language solutions that combine conversational language understanding models and question answering knowledge bases.

# Create a question answering knowledge base

1. Create an **Azure AI Language** resource in your Azure subscription.
    1. Enable the *question answering* feature.
    2. Create or select an **Azure Cognitive Search** resource to host the knowledge base index.
2. In Azure AI Language Studio, select the Language resource and create a **Custom question answering** project.
3. Name the knowledge base.
4. Add one or more data sources to populate the knowledge base:
    1. URLs for web pages containing FAQs.
    2. Files containing structured text from which questions and answers can be derived.
    3. Pre-defined *chit-chat* datasets that include common conversational questions and responses in a specified style.
5. Create the knowledge base and edit question and answer pairs in the portal.

# Multi-turn conversation

Although you can often create an effective knowledge base that consists of individual question and answer pairs, sometimes you might need to ask follow-up questions to elicit more information from a user before presenting a definitive answer. This kind of interaction is referred to as a *multi-turn* conversation.

You can enable multi-turn responses when importing questions and answers from an existing web page or document based on its structure, or you can explicitly define follow-up prompts and responses for existing question and answer pairs.

# Testing a knowledge base

You can test your knowledge base interactively in Azure AI Language Studio, submitting questions and reviewing the answers that are returned. You can inspect the results to view their confidence scores as well as other potential answers.

![[test-qna.png]]

# Deploy and query a knowledge base

When you are happy with the performance of your knowledge base, you can deploy it to a REST endpoint that client applications can use to submit questions and receive answers.

To consume the published knowledge base, you can use the REST interface.

The minimal request body for the function contains a question, like this:

```
{
  "question": "What do I need to do to cancel a reservation?"
}
```

The response includes the closest question match that was found in the knowledge base, along with the associated answer, the confidence score, and other metadata about the question and answer pair.

```
{
  "answers": [
    {
      "questions": [
        "How can I cancel a reservation?"
      ],
      "answer": "Call us on 555 123 4567 to cancel a reservation.",
      "confidenceScore": 1.0,
      "id": 6,
      "source": "https://margies-travel.com/faq",
      "metadata": {},
      "dialog": {
        "isContextOnly": false,
        "prompts": []
      }
    }
  ]
}
```

# Improve question answering performance

After creating and testing a knowledge base, you can improve its performance with *active learning* and by defining *synonyms*.

## Use active learning

**Active learning** can help you make continuous improvements so that it gets better at answering user questions correctly over time.

Active learning helps improve the knowledge base in two ways:

-   **Implicit feedback**: As incoming requests are processed, the service identifies user-provided questions that have multiple, similarly scored matches in the knowledge base. These are automatically clustered as alternate phrase suggestions for the possible answers that you can accept or reject in the **Suggestions** page for your knowledge base in Azure AI Language Studio.
-   **Explicit feedback**. When developing a client application you can control the number of possible question matches returned for the user's input by specifying the **top** parameter, as shown here:

```
{
    "question": "I want to book a hotel.",
    "top": 3
}
```

The response from the service includes a **question** object for each possible match, up to the **top** value specified in the request:

```
{
    "answers":[
        {"questions":[
            "How do I book a hotel?"
        ],
        "answer":"Call 555-123-4567 to book.",
        "score":76.55,
        "id":2,
        ...
        },
        {"questions":[
            "Can I book multiple hotel rooms?"
        ],
        "answer":"Yes, you can reserve up to 3 rooms.",
        "score":76.15,
        "id":6,
        ...
        },
        {"questions":[
            "Is there a booking fee?"
        ],
        "answer":"No, we do not charge a booking fee.",
        "score":75.99,
        "id":11,
        ...
        }
    ]
}
```

You can implement logic in your client app to compare the **score** property values for the questions, and potentially present the questions to the user so they can positively identify the question closest to what they intended to ask.

With the correct question identified, your app can use the REST API to send feedback containing suggested alternative phrasing based on the user's original input:

```
{
    "records": [
        {
            "userId": "1234",
            "userQuestion": "I want to book a hotel.",
            "qnaId": 2
        }
    ]
}
```

The **qnaId** in the feedback corresponds to the **id** of the question the user identified as the correct match. The **userId** parameter is an identifier for the user and can be any value you choose, such as an email address or numeric identifier.

The feedback will be presented in the active learning **Suggestions** page for your knowledge base in Azure AI Language Studio for you to accept or reject.

> To learn more about active learning, see the [Enrich your project with active learning](https://learn.microsoft.com/en-us/azure/cognitive-services/language-service/question-answering/tutorials/active-learning)

## Define synonyms

Synonyms are useful when question submitted by users might include multiple different words to mean the same thing. For example, a travel agency customer might refer to a "reservation" or a "booking". By defining these as synonyms, the question answering service can find an appropriate answer regardless of which term an individual customer uses.

To define synonyms, you must use the REST API to submit synonyms in the following JSON format:

```
{
    "synonyms": [
        {
            "alterations": [
                "reservation",
                "booking",,
                ]
        }
    ]
}
```

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/12-qna-maker.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/12-qna-maker.html)
