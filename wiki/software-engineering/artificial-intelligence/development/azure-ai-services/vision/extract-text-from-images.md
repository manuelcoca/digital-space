---
tags: azure, azureai, azureaivision
---

## Introduction

Azure AI Vision has Optical Character Recognition (OCR) possibility which allows to extract text from images, such as photos of street signs and products, as well as from documents — invoices, bills, financial reports, articles, and more.

The **Azure AI Vision** service offers two APIs that you can use to read text.

-   The **Read** API:
    -   Use this API to read small to large volumes of text from images and PDF documents.
    -   This API uses a newer model than the OCR API, resulting in greater accuracy.
    -   The Read API can read printed text in multiple languages, and handwritten text in English.
    -   The initial function call returns an asynchronous operation ID, which must be used in a subsequent call to retrieve the results.
-   The **Image Analysis** API:
    -   Currently still in preview, with reading text functionality added version 4.0.
    -   Use this API to read small amounts of text from images.
    -   Returns contextual information, including line number and position.
    -   Results are returned immediately (synchronous) from a single function call.
    -   Has functionality for analyzing images past extracting text, including detecting content (such as brands, faces, and domain-specific content), describing or categorizing an image, generating thumbnails and more.

Both can be used via the REST API or SDK.

## Read API

To use the Read API, call the **Read** REST function (or equivalent SDK method), passing the image URL or binary data, and optionally specifying the language the text is written in (with a default value of **en** for English).

The Read function returns an operation ID, which you can use in a subsequent call to the **Get Read Results** function in order to retrieve details of the text that has been read. Depending on the volume of text, you may need to poll the **Get Read Results** function multiple times before the operation is complete.

The results of from the Read API are broken down by *page*, then *line*, and then *word*. Additionally, the text values are included at both the *line* and *word* levels, making it easier to read entire lines of text if you don't need to extract text at the individual *word* level.

```
{
  "status": "succeeded",
  "createdDateTime": "2019-10-03T14:32:04Z",
  "lastUpdatedDateTime": "2019-10-03T14:38:14Z",
  "analyzeResult": {
    "version": "v3.0",
    "readResults": [
      {
        "page": 1,
        "language": "en",
        "angle": 49.59, "width": 600,
        "height": 400, "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              20,61,204,64,204,84,20,81],
            "text": "Hello world!",
            "words": [
              {
                "boundingBox": [
                  20,62,48,62,48,83,20,82],
                "text": "Hello",
                "confidence": 0.91
              },
              {
                "boundingBox": [
                  51,62,105,63,105,83,51,83],
                "text": "world!",
                "confidence": 0.164
              }
            ]
          }
        ]
      }
    ]
  }
}
```

---

## Resources

-   Trainings Resources: https://microsoftlearning.github.io/AI-102-AIEngineer/Instructions/20-ocr.html
