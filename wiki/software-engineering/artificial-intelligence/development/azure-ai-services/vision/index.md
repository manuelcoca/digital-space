---
tags: azure, azureai, azureaivision
---

The **Azure AI Vision** service is designed to help you extract information from images. It provides functionality that you can use for:

-   **Description and tag generation** - determining an appropriate caption for an image, and identifying relevant "tags" that can be used as keywords to indicate its subject.
-   **Object detection** - detecting the presence and location of specific objects within the image.
-   **Face detection** - detecting the presence, location, and features of human faces in the image.
-   **Image metadata, color, and type analysis** - determining the format and size of an image, its dominant color palette, and whether it contains clip art.
-   **Category identification** - identifying an appropriate categorisation for the image, and if it contains any known landmarks.
-   **Brand detection** - detecting the presence of any known brands or logos.
-   **Moderation rating** - determine if the image includes any adult or violent content.
-   **Optical character recognition** - reading text in the image.
-   **Smart thumbnail generation** - identifying the main region of interest in the image to create a smaller "thumbnail" version.

![[computer-vision.png]]

## Azure AI Custom Vision

Custom Vision is a service for analysing advanced images based on a custom model. Features:

-   [[Image Classification]]
-   [[Object Detection]]

How custom models can be trained and used:

1. Use existing (labeled) images to train an Azure AI Custom Vision model.
2. Create a client application that submits new images to your model to generate predictions.

To use the Azure AI Custom Vision service, you must provision two kinds of Azure resource:

-   A *training* resource (used to train your models). This can be:
    -   An **Azure AI Services** resource.
    -   An **Azure AI Custom Vision (Training)** resource.
-   A *prediction* resource, used by client applications to get predictions from your model. This can be:
    -   An **Azure AI Services** resource.
    -   An **Azure AI Custom Vision (Prediction)** resource.
