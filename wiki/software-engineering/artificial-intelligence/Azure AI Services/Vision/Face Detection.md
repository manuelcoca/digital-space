---
tags: azure, azureai, azureaivision
---

To detect and analyse faces with the Azure AI Vision service, call the **Analyze Image** REST function (or equivalent SDK method), specifying **Faces** as one of the visual features to be returned.

In images that contain one or more faces, the response includes details of their location in the image and the attributes of the detected person, like this:

```
{
  "faces": [
      {
        "faceRectangle": {
          "top": 225,
          "left": 237,
          "width": 130,
          "height": 130
        }
      },
      {
        "faceRectangle": {
          "top": 309,
          "left": 534,
          "width": 119,
          "height": 119
      }
    }
  ]
}
```

For more information on the Azure AI Vision endpoint, see the [Azure AI Vision REST API reference page](https://learn.microsoft.com/en-us/rest/api/computer-vision/)
