---
tags: azure, azureai, azureaivision
---

Image analyzation with Azure AI Vision can be done via REST or SDK.

The JSON response for image analysis looks similar to this example:

```
{
  "categories": [
   {
     "name": "_outdoor_mountain",
     "confidence": "0.9"}
  ],
  "adult": {"isAdultContent": "false", …},
  "tags": [
    {"name": "outdoor", "confidence": 0.9},
    {"name": "mountain", " confidence ": 0.9}],
  "description": {
    "tags":["outdoor", "mountain"],
    "captions": [
      {"name": "A mountain with snow",
       "confidence": 0.9
      }
    ]
  },
  "metadata":
    {"width":60,"height":30, format:"Jpeg"},
  "faces": [],
  "brands": [],
  "color": {"dominantColorForeground": "Brown",…},
  "imageType": {"clipArtType": 0, …},
  "objects" : [
    {
     "rectangle": {x:20, y:25, w:10, h:20},
     "object": "mountain",
     "confidence": 0.9
    }
  ]
}
```

# Generate a smart-cropped thumbnail

-   Thumbnails are commonly used to show smaller versions of images in applications and websites.
-   For instance, a tourism site may use thumbnails for tourist attractions, only showing the full image when a user selects "details" for a particular attraction.
-   The Azure AI Vision service allows creating thumbnails with different dimensions and aspect ratios from source images.
-   It can use image analysis to identify the main subject (region of interest) and focus on it, particularly helpful when changing the aspect ratio by cropping the image.

![[smart-cropping.png]]

You can generate thumbnails with a width and height up to 1024 pixels, with a recommended minimum size of 50x50 pixels.

---

# Resources

-   Training Resource: [https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/15-computer-vision.html](https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/15-computer-vision.html)
