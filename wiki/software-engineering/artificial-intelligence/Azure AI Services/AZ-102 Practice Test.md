---
tags: azure, azureai
---

# Topics for Exam

## Plan and manage an Azure AI solution (15–20%)

#### Select the appropriate Azure AI service

-   Select the appropriate service for a computer vision solution
-   Select the appropriate service for a natural language processing solution
-   Select the appropriate service for a decision support solution
-   Select the appropriate service for a speech solution
-   Select the appropriate service for a generative AI solution
-   Select the appropriate service for a document intelligence solution
-   Select the appropriate service for a knowledge mining solution

#### Plan, create and deploy an Azure AI service

-   Plan for a solution that meets Responsible AI principles
-   Create an Azure AI resource
-   Determine a default endpoint for a service
-   Integrate Azure AI services into a continuous integration and continuous delivery (CI/CD) pipeline
-   Plan and implement a container deployment

#### Manage, monitor and secure an Azure AI service

-   Configure diagnostic logging
-   Monitor an Azure AI resource
-   Manage costs for Azure AI services
-   Manage account keys
-   Protect account keys by using Azure Key Vault
-   Manage authentication for an Azure AI Service resource
-   Manage private communications

https://learn.microsoft.com/en-gb/credentials/certifications/resources/study-guides/ai-102#skills-measured-as-of-october-31-2023

## Implement decision support solutions (10–15%)

#### Create decision support solutions for data monitoring and anomaly detection

-   Implement a univariate anomaly detection solution with Azure AI Anomaly Detector
-   Implement a multivariate anomaly detection solution Azure AI Anomaly Detector
-   Implement a data monitoring solution with Azure AI Metrics Advisor

#### Create decision support solutions for content delivery

-   Implement a text moderation solution with Azure AI Content Safety
-   Implement an image moderation solution with Azure AI Content Safety
-   Implement a content personalization solution with Azure AI Personalizer

## Implement computer vision solutions (15–20%)

#### Analyze images

-   Select visual features to meet image processing requirements
-   Detect objects in images and generate image tags
-   Include image analysis features in an image processing request
-   Interpret image processing responses
-   Extract text from images using Azure AI Vision
-   Convert handwritten text using Azure AI Vision

#### Implement custom computer vision models by using Azure AI Vision

-   Choose between image classification and object detection models
-   Label images
-   Train a custom image model, including image classification and object detection
-   Evaluate custom vision model metrics
-   Publish a custom vision model
-   Consume a custom vision model

#### Analyze videos

-   Use Azure AI Video Indexer to extract insights from a video or live stream
-   Use Azure AI Vision Spatial Analysis to detect presence and movement of people in video

## Implement natural language processing solutions (30–35%)

#### Analyze text by using Azure AI Language

-   Extract key phrases
-   Extract entities
-   Determine sentiment of text
-   Detect the language used in text
-   Detect personally identifiable information (PII) in text

#### Process speech by using Azure AI Speech

-   Implement text-to-speech
-   Implement speech-to-text
-   Improve text-to-speech by using Speech Synthesis Markup Language (SSML)
-   Implement custom speech solutions
-   Implement intent recognition
-   Implement keyword recognition

#### Translate language

-   Translate text and documents by using the Azure AI Translator service
-   Implement custom translation, including training, improving, and publishing a custom model
-   Translate speech-to-speech by using the Azure AI Speech service
-   Translate speech-to-text by using the Azure AI Speech service
-   Translate to multiple languages simultaneously

#### Implement and manage a language understanding model by using Azure AI Language

-   Create intents and add utterances
-   Create entities
-   Train, evaluate, deploy, and test a language understanding model
-   Optimize a language understanding model
-   Consume a language model from a client application
-   Backup and recover language understanding models

#### Create a question answering solution by using Azure AI Language

-   Create a question answering project
-   Add question-and-answer pairs manually
-   Import sources
-   Train and test a knowledge base
-   Publish a knowledge base
-   Create a multi-turn conversation
-   Add alternate phrasing
-   Add chit-chat to a knowledge base
-   Export a knowledge base
-   Create a multi-language question answering solution

## Implement knowledge mining and document intelligence solutions (10–15%)

#### Implement an Azure Cognitive Search solution

-   Provision a Cognitive Search resource
-   Create data sources
-   Create an index
-   Define a skillset
-   Implement custom skills and include them in a skillset
-   Create and run an indexer
-   Query an index, including syntax, sorting, filtering, and wildcards
-   Manage Knowledge Store projections, including file, object, and table projections

#### Implement an Azure AI Document Intelligence solution

-   Provision a Document Intelligence resource
-   Use prebuilt models to extract data from documents
-   Implement a custom document intelligence model
-   Train, test, and publish a custom document intelligence model
-   Create a composed document intelligence model
-   Implement a document intelligence model as a custom Azure Cognitive Search skill

## Implement generative AI solutions (10–15%)

#### Use Azure OpenAI Service to generate content

-   Provision an Azure OpenAI Service resource
-   Select and deploy an Azure OpenAI model
-   Submit prompts to generate natural language
-   Submit prompts to generate code
-   Use the DALL-E model to generate images
-   Use Azure OpenAI APIs to submit prompts and receive responses

#### Optimize generative AI

-   Configure parameters to control generative behavior
-   Apply prompt engineering techniques to improve responses
-   Use your own data with an Azure OpenAI model
-   Fine-tune an Azure OpenAI model

# Question and Answers

| Question                                                                                                                                                                                                                                                                                                                                                                                            | Answer                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What is the maximum character count in each document that you send to the Key Phrase Extraction service?                                                                                                                                                                                                                                                                                            | 50,000 characters                                        | The maximum size of a record should be 50,000 characters as measured by String.Length. If you need to break up your data before sending it to the key phrase extractor, consider using the Text Split skill. If you do use a text split skill, set the page length to 5000 for the best performance.                                                                                                                                                                                                                                                                        |
| Identify the missing word(s) in the following sentence within the context of Microsoft Azure. A(n) [?] represents a continuous segment of the video. Transitions within the video are detected which determine how the video is split up. There can be gaps in time between the [?]s but together they are representative of the shot                                                               | Keyframe                                                 | `**Keyframes**` are frames that represent the shot. Each one is for a specific point in time. There can be gaps in time between keyframes but together they are representative of the shot. Each keyframe can be downloaded as a high-resolution image.                                                                                                                                                                                                                                                                                                                     |
| You have an app named App1 that extracts invoice data from PDF files by using an S0 instance of Azure AI Document Intelligence. The PDF files are up to 2 MB each and contain up to 10 pages. Users report that App1 is unable to process some invoices. You need to troubleshoot the issue. What is a possible cause of the issue?                                                                 | Some of the files are password protected                 | Although file size and number of pages can cause failures, the limit for the S0 tier is 500 MB and 2,000 pages. The S0 tier is sufficient for the file characteristics mentioned.                                                                                                                                                                                                                                                                                                                                                                                           |
| You are building an app that will be deployed to an edge device and will use Azure AI Custom Vision to analyze images of fruits. You need to select a model domain for the app. The solution must support running the app without internet connectivity. Which model should you use?                                                                                                                | Compact Domain                                           | Compact Domain is the only model to export and optimized to use on mobile devices                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| You are building an app that will extract data from legal documents. You plan to use the Azure AI Document Intelligence prebuilt-read model. Which three document formats does the prebuilt-read model support? Each correct answer presents a complete solution. Select all answers that apply.                                                                                                    | Word, Excel, PDF                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| You are using Azure AI Bot Service to build a bot that will assist employees with work-related tasks. You need to ensure that the bot is accessible only from the company network. What should you use?                                                                                                                                                                                             | Direct Line App Service Extension                        | The only way to limit the access to an Azure AI bot service is to use the Direct Line App Service extension, which will allow you to limit access to the service to a specific (private) network.                                                                                                                                                                                                                                                                                                                                                                           |
| You need to build a solution that will detect anomalies in a real-time data stream. Which anomaly detection model should you use?                                                                                                                                                                                                                                                                   | latest point                                             | For real-time data monitoring, it is recommended that you detect the Azure AI anomaly status of your latest data point only.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Your company has multiple business units spread across six geographic regions. You are building an app that will detect anomalies in revenue data from the business units. You need to configure the solution to detect anomalies in the data based on correlations across the regions. Which feature of Azure AI Anomaly Detector should you use?                                                  | Multivariate Anomaly Detection                           | Multivariate Anomaly Detection is designed to detect anomalies in multiple variables with correlations, which are usually gathered from equipment or other complex systems. The underlying model used is a Graph Attention Network.https://learn.microsoft.com/en-us/azure/ai-services/anomaly-detector/overview                                                                                                                                                                                                                                                            |
| You have an app named App1 that analyzes social media mentions and determines whether comments are positive or negative. During testing, you notice that App1 generates negative sentiment analysis in response to customer feedback that contains positive feedback. You need to ensure that App1 includes more granular information during the analysis. What should you add to the API requests? | opinionMining=true                                       | https://learn.microsoft.com/azure/cognitive-services/language-service/sentiment-opinion-mining/how-to/call-api                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| You are building an app that will analyze meeting recordings and identify who is speaking at which moment in time. You need to configure a voice profile for the app. Which type of voice profile should you use?                                                                                                                                                                                   | text-independent verification                            | Text-independent verification means that speakers can speak in everyday language in enrollment and verification phases. This is the voice profile that should be used when configuring a voice profile for the app. Text-dependent verification means that speakers need to choose the same passphrase to use during both enrollment and verification phases. Speaker identification helps you determine an unknown speaker’s identity within a group of enrolled speakers.https://learn.microsoft.com/azure/cognitive-services/speech-service/speaker-recognition-overview |
| You are building an app that will enable users to create notes by using speech. You need to recommend the Azure AI Speech service model to use. The solution must support noisy environments. Which model should you recommend?                                                                                                                                                                     | custom speech to text.                                   | The custom speech-to-text model is correct, as you need to adapt the model because a factory floor might have ambient noise, which the model should be trained on.                                                                                                                                                                                                                                                                                                                                                                                                          |
| You have a question answering project. A customer asks a question that is not part of the project. You review the active learning suggestions and do not see any suggestions. You need to ensure that customer questions are included in the active learning suggestions. The solution must minimize administrative effort.                                                                         | Wait at least 30 minutes before checking for suggestions | Active learning suggestions are not in real time. There is an approximate delay of 30 minutes before suggestions show on this pane. This delay balances the high cost involved in real-time updates to the index and service performance. Active learning is turned on by default. You can use active learning for this instead of manually logging the questions and adding them. https://learn.microsoft.com/azure/cognitive-services/language-service/question-answering/tutorials/active-learning                                                                       |
| You are building a solution that uses Azure Cognitive Search. You need to create a skillset definition. What are minimum sections you should include in the definition?Select only one answer.                                                                                                                                                                                                      | name, description and skills                             | https://learn.microsoft.com/azure/search/cognitive-search-defining-skillset#add-a-skillset-definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

# Resources

-   Udemy Practice Tests
    -   https://www.udemy.com/course/microsoft-certified-azure-ai-engineer-associate-ai-102/
    -   https://www.udemy.com/course/ai-102-microsoft-azure-ai-solution-exam/

# Recheck topics

-   Pricing Plans/Instances
-   Azure AI Vison Model Domains
-   Anonmaly Detection models
-   Voice Profiles
-   AI Speech models
-   Bot Service??
