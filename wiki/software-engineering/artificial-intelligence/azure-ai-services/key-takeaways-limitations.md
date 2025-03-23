---
tags: azure, azureai
---

# Language

-   Pricing is based on submitted text records
    -   1 text record = 1000 characters
-   Data and rate limits depend on number of documents
    -   If limit exceeded break document into smaller chunks
    -   A document is basically a string of text characters
-   Maximum chars per document:
    -   Text analytics for health = 125.000 chars
    -   All other preconfigured features:
        -   Synchronous: 5120 chars
        -   Async: 125.000 chars
-   Maximum request size: 1 MB
-   Rate limits:
    -   S / Multi service = 1000 per second and minute
    -   S0 / F0 = 100 per second and 300 per minute
-   Max documents per request:

| Feature                                            | Max Documents Per Request                                                       |
| -------------------------------------------------- | ------------------------------------------------------------------------------- |
| Conversation summarization                         | 1                                                                               |
| Language Detection                                 | 1000                                                                            |
| Sentiment Analysis                                 | 10                                                                              |
| Opinion Mining                                     | 10                                                                              |
| Key Phrase Extraction                              | 10                                                                              |
| Named Entity Recognition (NER)                     | 5                                                                               |
| Personally Identifying Information (PII) detection | 5                                                                               |
| Document summarization                             | 25                                                                              |
| Entity Linking                                     | 5                                                                               |
| Text Analytics for health                          | 25 for the web-based API, 1000 for the container. (125,000 characters in total) |

# Translator

-   Over 100 languages
-   Character limits/Pricing tiers:
    -   F0 - 2 million characters per hour
    -   S1 - 40 million char per hour
    -   S2 / C2 - 40 million char per hour
    -   S3 / C3 - 120 million char per hour
    -   S4 / C4 - 200 million char per hour
-   Character limits per request:
    -   Translate and Detect: max. 50.000 chars
    -   Transliterate: max. 5000 chars
    -   Dictionary Lookup: 1000 chars
-   Maximum latency of 15 seconds using standard models and 120 seconds when using custom models.
    -   Typically, responses *for text within 100 characters* are returned in 150 milliseconds to 300 milliseconds.
-   Document translation file limitations:
    -   File size: max. 40MB
    -   Total number of files: max. 1000
    -   Total batch content size: max .250MB
    -   Number of target languages in batch: max. 10
    -

# Vision

Pricing options:

-   Free - 20 transactions per minute - 5,000 free transactions per month
-   S1 - 30 read transactions per second or 15 non read transactions - CHF 0.879 per 1,000 transactions. Features:
    -   Tag, Face, GetThumbnail, Colour, Image Type, GetAreaOfInterest, People Detection, Smart Crops
-   S2 - Same transactions and price, but other features:
    -   OCR, Adult, Celebrity, Landmark,Detect, objects, Brand
-   S3 - Same transactions - CHF 1.318 per 1,000 transactions . Features:
    -   Describe, Read, Caption, Dense Caption
-   Standard - CHF 0.0095 per hour. Features - Spatial Analysis on Edge Image Analysis:
-   Version 4 and 3 available
    -   v3 has more features:
        -   Tags, Objects, Descriptions, Brands, Faces, Image type, Color scheme, Landmarks, Celebrities, Adult content, Smart crop
    -   v4 has better models but not all features are yet supported by v4:
        -   Read text, Captions, Dense captions, Tags, Object detection, Custom image classification / object detection, People, Smart crop
    -   Use 4 if feature is already supported, otherwise use v3
-   Image limitations
    -   v4
        -   Format: JPEG, PNG, GIF, BMP, WEBP, ICO, TIFF, or MPO
        -   File size: less than 20 megabytes (MB)
        -   Dimensions greater than 50 x 50 pixels and less than 16,000 x 16,000 pixels
    -   v3
        -   Format: JPEG, PNG, GIF, or BMP format
        -   File size: less than 4 MB
        -   Dimensions greater than 50 x 50 pixels and less than 16,000 x 16,000 pixels
-   Visual Image Features like detecting Brands, Colors, Description etc. are only available in english OCR: - Language Support: - Handwritten text is only available in 5 languages: English, Chinese, French, German, Italian - Print text is available for around 70 languages Spatial Analysis:
-   Detect the presence and movements of people in video
-   Features:
    -   People counting - count how many people join a specific area
    -   Entrance Counting - counting how long someone is in a specific area
    -   Social distancing and face mask detection
-   Video limitations:
    -   Format: RTSP, rawvideo, MP4, FLV, or MKV.
    -   Video codec: H.264, HEVC(H.265), rawvideo, VP9, or MPEG-4.

# Azure Document Intelligence

-   Prebuilt Models:
    -   Invoice
    -   Receipt
    -   W-2 US tax declaration
    -   ID Document
    -   Business card
    -   Health insurance card
-   Pricing Options:
    -   Free - All document types - 500 pages per month
    -   S0 - Read - 1.318 CHF per 1000 pages
    -   S0 - Prebuilt models - 8.784 CHF per 1000 pages
-   Limitations:

| Document types supported                | Read | Layout | Prebuilt models | Custom models |
| --------------------------------------- | ---- | ------ | --------------- | ------------- |
| PDF                                     | ✔️   | ✔️     | ✔️              | ✔️            |
| Images (JPEG/JPG), PNG, BMP, TIFF, HEIF | ✔️   | ✔️     | ✔️              | ✔️            |
| Office file types DOCX, PPT, XLS        | ✔️   | ✔️     | ✖️              | ✖️            |

| Quota                              | Free (F0)1 | Standard (S0)      |
| ---------------------------------- | ---------- | ------------------ |
| **Transactions Per Second limit**  | 1          | 15 (default value) |
| Adjustable                         | No         | Yes 2              |
| **Max document size**              | 4 MB       | 500 MB             |
| Adjustable                         | No         | No                 |
| **Max number of pages (Analysis)** | 2          | 2000               |
| Adjustable                         | No         | No                 |
| **Max size of labels file**        | 10 MB      | 10 MB              |
| Adjustable                         | No         | No                 |
| **Max size of OCR json response**  | 500 MB     | 500 MB             |
| Adjustable                         | No         | No                 |
| **Max number of Template models**  | 500        | 5000               |
| Adjustable                         | No         | No                 |
| **Max number of Neural models**    | 100        | 500                |
| Adjustable                         | No         | No                 |

# AI Personalizer

-   There are 2 learning modes:
    -   Apprentice mode
    -   Online Mode
-   Once the Reward achievement is over 75-80% change to online mode. Following ratios are available:
    -   **Baseline – average reward**: Average reward of the application’s default business logic (baseline).
    -   **Personalizer – average reward**: Average of total reward your AI Personalizer could potentially have reached.
    -   **Reward achievement ratio over most recent 1000 events**: A ratio that represents the Baseline and Personalizer reward.

# Content Safety

-   Text classification/moderation and image moderation available
-   Harm categories:
    -   Hate and Fairness
    -   Sexual
    -   Violence
    -   Self-harm
-   Each response has a harm category and severity level (how harmful is it)
    -   0 (not harmful) - 7 (harmful)
-   Finding personal data is limited to:
    -   Email addresses
    -   US mailing addresses
    -   IP addresses
    -   US phone numbers
    -   UK phone numbers
    -   Social Security numbers
-   Text Classification available in over 70 languages
-   Image moderation file limitations:
    -   Format: min. 50px x 50px and max. 2048 px x 2048 px
    -   File size: max. 4MB
-   Pricing Options
    -   Free
        -   5000 text records per month
        -   5000 images per month
    -   Standard
        -   0.66 CHF per 1000 text records
        -   1.32 CHF per 1000 images

# Anomaly Detector

-   Univariate Anomaly Detector
    -   Detect anomalies in one variable, like revenue, cost, etc. The model was selected automatically based on your data pattern.
-   Multivariate Anomaly Detector
    -   Detect anomalies in multiple variables with correlations, which are usually gathered from other complex system. The underlying model used is a Graph Attention Network.
-   Pricing Options:
    -   Free and Standard Option
        -   Free Univariate = 20.000 transactions per month free
        -   Free Multivariate = 5 hours of training free
        -   Standard Univariate = 0.276 CHF per 1000 transactions
        -   Standard Multivariate = 0.211 CHF per training hour
