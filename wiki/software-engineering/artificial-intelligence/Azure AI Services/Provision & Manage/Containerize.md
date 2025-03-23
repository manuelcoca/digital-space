---
tags:
    - azure
    - azureai
    - azurecontainer
    - azureaci
    - azureacr
---

Azure AI Services can be deployed in containers on-premise or in Azure Cloud. If your application uses sensitive data in an on-premises SQL Server to call an Azure AI service, you can deploy Azure AI Services in containers on the same network:

-   Sensitive data is not passed through cloud
-   On-Premise can also decrease latency between services (if other services also running on premise)

# How it works

1. Provision an Azure AI Service
    1. Even when using a container, you must provision an Azure AI services resource in Azure for billing purposes
2. Download container image and run on local docker host, ACI, AKS or somewhere else
3. When container endpoints are consumed, the container will sent usage metrics to the provisioned Azure AI Cloud Service in order to calculate billing

# Container images

Each container provides a subset of Azure AI Services functionality. For example, not all features of the Language service are in a single container. Language detection, translation, and sentiment analysis are each separate container images. However, the setup steps are similar for each container.

For a full list of currently available Azure AI Services container images, and specific notes for each one, see [Azure AI Services container images](https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-container-support).

## Language Containers

| Feature                         | Image                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------ |
| Key Phrase Extraction           | mcr.microsoft.com/azure-cognitive-services/textanalytics/keyphrase                   |
| Language Detection              | mcr.microsoft.com/azure-cognitive-services/textanalytics/language                    |
| Sentiment Analysis v3 (English) | mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-en            |
| Text Language Detection         | mcr.microsoft.com/product/azure-cognitive-services/textanalytics/language/about      |
| Text Analytics for health       | mcr.microsoft.com/product/azure-cognitive-services/textanalytics/healthcare/about    |
| Translator                      | mcr.microsoft.com/product/azure-cognitive-services/translator/text-translation/about |

## Speech Containers

| Feature                   | Image                                                                                         |
| ------------------------- | --------------------------------------------------------------------------------------------- |
| Speech to text            | mcr.microsoft.com/product/azure-cognitive-services/speechservices/speech-to-text/about        |
| Custom Speech to text     | mcr.microsoft.com/product/azure-cognitive-services/speechservices/custom-speech-to-text/about |
| Neural Text to speech     | mcr.microsoft.com/product/azure-cognitive-services/speechservices/neural-text-to-speech/about |
| Speech language detection | mcr.microsoft.com/product/azure-cognitive-services/speechservices/language-detection/about    |

## Vision Containers

| Feature          | Image                                                                            |
| ---------------- | -------------------------------------------------------------------------------- |
| Read OCR         | mcr.microsoft.com/product/azure-cognitive-services/vision/read/about             |
| Spatial analysis | mcr.microsoft.com/product/azure-cognitive-services/vision/spatial-analysis/about |

# Container Configuration

When you deploy an Azure AI Services container image to a host, you must specify three settings.

| Setting | Description                                                            |
| ------- | ---------------------------------------------------------------------- |
| ApiKey  | Key from your deployed Azure AI Service; used for billing.             |
| Billing | Endpoint URI from your deployed Azure AI Service; used for billing.    |
| Eula    | Value of **accept** to state you accept the license for the container. |

# Example running Azure AI Language Understanding

Download the Language Understanding container image, like this:

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/language:latest
```

Run the local Docker instance:

```
docker run --rm -it -p 5000:5000 
--memory 4g 
--cpus 1 \
mcr.microsoft.com/azure-cognitive-services/textanalytics/language \
Eula=accept \
Billing={ENDPOINT} \
ApiKey={API_KEY}
```

| Placeholder  | Value                                                                                                                     |
| ------------ | ------------------------------------------------------------------------------------------------------------------------- |
| `{ENDPOINT}` | The endpoint for submitting usage information, used for billing. This value can be found in the resource **Overview**tab. |
| `{API_KEY}`  | The API key for the resource, found on the **Keys** tab.                                                                  |
