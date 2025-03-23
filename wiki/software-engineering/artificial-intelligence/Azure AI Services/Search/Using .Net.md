---
tags: azure, azureai, azureaidocumentintelligence
---

Two configs needed to connect to Resource:

-   **Endpoint.** This is the URL where the resource can be contacted.
-   **Access key.** This is unique string that Azure uses to authenticate the call to Azure AI Document Intelligence.

```
using Azure;
using Azure.AI.FormRecognizer.DocumentAnalysis;

string endpoint = "<endpoint>";
string key = "<access-key>";
AzureKeyCredential cred = new AzureKeyCredential(key);
DocumentAnalysisClient client = new DocumentAnalysisClient(new Uri(endpoint), cred);

//sample form document
Uri fileUri = new Uri ("<url-of-document-to-analyze>");

AnalyzeDocumentOperation operation = await client.StartAnalyzeDocumentFromUriAsync("prebuilt-document", fileUri);

await operation.WaitForCompletionAsync();

AnalyzeResult result = operation.Value;
```
