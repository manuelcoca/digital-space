---
tags: azure, azureai, azureaisearch
---

If predefined skills doesn't meet requirements, custom skills can be implemented to meet specific data extraction needs. Each Custom Skill can be a call to an other AI Service.

There are two types of custom skill that you can create:

-   **Azure Machine Learning (AML) custom skills.** You can use this custom skill type to enrich your index by calling an AML model.
-   **Custom Web API skills.** You can use this custom skill type to enrich your index by calling a web service. Such web services can include Azure applied AI services, such as Azure AI Document Intelligence.

## Create a custom skill

Your custom skill must implement the expected schema for input and output data that is expected by skills in an Azure Cognitive Search skillset.

Call REST API to create a Skillset:

```
PUT https://[service name].search.windows.net/skillsets/[skillset name]?api-version=[api version]
  Content-Type: application/json
  api-key: [admin key]
```

In the above call:

-   [service name] is the name of your Cognitive Search service in Azure.
-   [skillset name] is a name for the skillset you're creating e.g **Custom.WebApiSkill**
-   [api version] is the version of the Search REST API.
-   [admin key] is the API key for the Search service. You can obtain this key from the Azure portal.

Body:

```
{
  "name" : "A name for the skillset",
  "description" : "Optionally, add a descriptive text.",
  "skills" : [
		"#Microsoft.Skills.Custom.WebApiSkill"
    ],
  "cognitiveServices":
      {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "key": "<admin key>"
      },
  "knowledgeStore": { ... },
  "encryptionKey": { }
}
```

Result: The skill definition must:

-   Specify the URI to your web API endpoint, including parameters and headers if necessary.
-   Set the context to specify at which point in the document hierarchy the skill should be called
-   Assign input values, usually from existing document fields
-   Store output in a new field, optionally specifying a target field name (otherwise the output name is used)

```
{
    "skills": [
      ...,
		{
		  "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
		  "description": "A custom skill",
		  "uri": "https://contoso.com/formrecognizer",
		  "batchSize": 1,
		  "context": "/document",
		  "inputs": [
		    {
		      "name": "formUrl",
		      "source": "/document/metadata_storage_path"
		    }
		  ],
		  "outputs":[
		    {
		      "name":"address",
		      "targetName":"address"
		    },
		    {
		      "name":"recipient",
		      "targetName":"recipient"
		    }
		  ]
		}
  ]
}
```

### Input Schema

The input schema for a custom skill defines a JSON structure containing a record for each document to be processed. Each document has a unique identifier, and a data payload with one or more inputs, like this:

```
{
    "values": [
      {
        "recordId": "<unique_identifier>",
        "data":
           {
             "<input1_name>":  "<input1_value>",
             "<input2_name>": "<input2_value>",
             ...
           }
      },
      {
        "recordId": "<unique_identifier>",
        "data":
           {
             "<input1_name>":  "<input1_value>",
             "<input2_name>": "<input2_value>",
             ...
           }
      },
      ...
    ]
}
```

## Output schema

The schema for the results returned by your custom skill reflects the input schema. It is assumed that the output contains a record for each input record, with either the results produced by the skill or details of any errors that occurred.

```
{
    "values": [
      {
        "recordId": "<unique_identifier_from_input>",
        "data":
           {
             "<output1_name>":  "<output1_value>",
              ...
           },
         "errors": [...],
         "warnings": [...]
      },
      {
        "recordId": "< unique_identifier_from_input>",
        "data":
           {
             "<output1_name>":  "<output1_value>",
              ...
           },
         "errors": [...],
         "warnings": [...]
      },
      ...
    ]
}
```

## Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/02-search-skills.html
