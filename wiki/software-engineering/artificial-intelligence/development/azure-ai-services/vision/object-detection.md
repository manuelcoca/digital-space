---
tags: azure, azureai, azureaivision
---

*Object detection* is a common computer vision problem that requires a custom model to identify the location of specific classes of object in an image.

## Understand object detection

*Object detection* is a form of computer vision in which a model is trained to detect the presence and location of one or more classes of object in an image. For example, an AI-enabled checkout system in a grocery store might need to identify the type and location of items being purchased by a customer.

![[object-detection.png]]

There are two components to an object detection prediction:

-   The class label of each object detected in the image. For example, you might ascertain that an image contains one apple and two oranges.
-   The location of each object within the image, indicated as coordinates of a *bounding box* that encloses the object.

## Train an object detector

The most significant difference between training an *image classification* model and training an *object detection* model is the labeling of the images with tags. While image classification requires one or more tags that apply to the whole image, object detection requires that each label consists of a tag and a *region* that defines the bounding box for each object in an image. The Azure AI Custom Vision portal provides a graphical interface that you can use to label your training images.

![[custom-vision-portal 1.png]]

## Bounding box measurement units

If you choose to use a labeling tool other than the Azure AI Custom Vision portal, you may need to adjust the output to match the measurement units expected by the Azure AI Custom Vision API. Bounding boxes are defined by four values that represent the left (X) and top (Y) coordinates of the top-left corner of the bounding box, and the width and height of the bounding box. These values are expressed as *proportional* values relative to the source image size. For example, consider this bounding box:

-   Left: 0.1
-   Top: 0.5
-   Width: 0.5
-   Height: 0.25

This defines a box in which the left is located 0.1 (one tenth) from the left edge of the image, and the top is 0.5 (half the image height) from the top. The box is half the width and a quarter of the height of the overall image.

The following image shows labeling information in JSON format for objects in an image.

![[object-labels.png]]
