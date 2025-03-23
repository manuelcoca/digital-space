# Introduction

Content moderation helps to verify text content in places like:

-   Chat rooms
-   Discussion boards
-   Chatbots
-   E-commerce catalogs
-   Documents

It allows to either block, approve or review the content based on custom policies and thresholds. The response from the Text Moderation API includes following informations:

-   A list of potentially unwanted words found in the text.
-   What type of potentially unwanted words were found.
-   Possible personal data found in the text.

# Profanity

When you pass text to the API, any potentially profane terms in the text are identified and returned in a JSON response. The profane item is returned as a `Term` in the JSON response, along with an index value showing where the term is in the supplied text.

You can also use custom term lists with this API. In that case, if a profane term is identified in the text, a `ListId` is also returned to identify the specific custom word that was identified. A sample JSON response:

```
"Terms": [
{
    "Index": 118,
    "OriginalIndex": 118,
    "ListId": 0,
    "Term": "crap"
}
```

# Classification

This feature of the API can place text into specific categories based on the following specifications:

-   **Category 1:** Potential presence of language that might be considered sexually explicit or adult in certain situations.
-   **Category 2:** Potential presence of language that might be considered sexually suggestive or mature in certain situations.
-   **Category 3:** Potential presence of language that might be considered offensive in certain situations.

When the JSON response is returned, it provides a Boolean value for a recommended review of the text. If `true`, you should review the content manually to determine the potential for any issues.

Each category is also returned with a score between 0 and 1 to indicate the predicted category for the evaluated text. The higher the score, the more likely it is that the category might apply. Here's a sample JSON response:

```
"Classification": {
    "ReviewRecommended": true,
    "Category1": {
        "Score": 0.99756889843889822
        },
    "Category2": {
        "Score": 0.12747249007225037
        },
    "Category3": {
        "Score": 0.98799997568130493
    }
}
```

# Personal Data

Personal data is of critical importance in many applications. This feature of the API can help you detect if any values in the text might be considered personal data before you release it publicly. Key aspects that are detected include:

-   Email addresses
-   US mailing addresses
-   IP addresses
-   US phone numbers
-   UK phone numbers
-   Social Security numbers

If possible personal data values are found, the JSON response includes relevant information about the text and the index location within the text. A sample JSON response is shown here:

```
"PII": {
    "Email": [{
        "Detected": "abcdef@abcd.com",
        "SubType": "Regular",
        "Text": "abcdef@abcd.com",
        "Index": 32
        }],
    "IPA": [{
        "SubType": "IPV4",
        "Text": "255.255.255.255",
        "Index": 72
        }],
    "Phone": [{
        "CountryCode": "US",
        "Text": "5557789887",
        "Index": 56
        }, {
        "CountryCode": "UK",
        "Text": "+44 123 456 7890",
        "Index": 208
        }],
    "Address": [{
        "Text": "1 Microsoft Way, Redmond, WA 98052",
        "Index": 89
        }],
    "SSN": [{
        "Text": "999-99-9999",
        "Index": 267
        }]
    }
```

---

# Resources

-   Training: https://learn.microsoft.com/en-us/training/modules/classify-and-moderate-text-with-azure-content-moderator/4-exercise-use-the-api-console
