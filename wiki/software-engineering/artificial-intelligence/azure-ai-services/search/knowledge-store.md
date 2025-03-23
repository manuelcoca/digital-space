---
tags: azureai, azureaisearch, azure
---

# What is a knowledge store?

While the index might be considered the primary output from an indexing process, the enriched data it contains might also be useful in other ways. For example:

-   Since the index is essentially a collection of JSON objects, each representing an indexed record, it might be useful to export the objects as JSON files for integration into a data orchestration process using tools such as Azure Data Factory.
-   You may want to normalize the index records into a relational schema of tables for analysis and reporting with tools such as Microsoft Power BI.
-   Having extracted embedded images from documents during the indexing process, you might want to save those images as files.

Azure Cognitive Search supports these scenarios by enabling you to define a knowledge store. The knowledge store consists of *projections* of the enriched data, which can be JSON objects, tables, or image files. When an indexer runs the pipeline to create or update an index, the projections are generated and persisted in the knowledge store.

# Define projections

Each skill in your skillset iteratively builds a JSON representation of the enriched data for the documents being indexed, and you can persist some or all of the fields in the document as projections. However, indexing create a complex document that don't map easily to well-formed JSON. To simplify the mapping of these field values to projections in a knowledge store, it's common to use the **Shaper** skill to create a new, field containing a simpler structure for the fields you want to map to projections.

For example, consider the following Shaper skill definition:

```
{
  "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
  "name": "define-projection",
  "description": "Prepare projection fields",
  "context": "/document",
  "inputs": [
    {
      "name": "file_name",
      "source": "/document/metadata_content_name"
    },
    {
      "name": "url",
      "source": "/document/url"
    },
    {
      "name": "sentiment",
      "source": "/document/sentimentScore"
    },
    {
      "name": "key_phrases",
      "source": null,
      "sourceContext": "/document/merged_content/keyphrases/*",
      "inputs": [
        {
          "name": "phrase",
          "source": "/document/merged_content/keyphrases/*"
        }
      ]
    }
  ],
  "outputs": [
    {
      "name": "output",
      "targetName": "projection"
    }
  ]
}
```

This Shaper skill creates a **projection** field with the following structure:

```
{
    "file_name": "file_name.pdf",
    "url": "https://<storage_path>/file_name.pdf",
    "sentiment": 1.0,
    "key_phrases": [
        {
            "phrase": "first key phrase"
        },
        {
            "phrase": "second key phrase"
        },
        {
            "phrase": "third key phrase"
        },
        ...
    ]
}
```

The resulting JSON is well-formed and can be mapped to a projection in a knowledge store.

# Define a knowledge store

To define the knowledge store and the projections you want to create in it, you must create a **knowledgeStore** object in the skillset that specifies the Azure Storage connection string for the storage account where you want to create projections, and the definitions of the projections themselves.

You can define object projections, table projections, and file projections depending on what you want to store; however note that you must define a separate *projection* for each type of projection, even though each projection contains lists for tables, objects, and files. Projection types are mutually exclusive in a projection definition, so only one of the projection type lists can be populated. If you create all three kinds of projection, you must include a projection for each type; as shown here:

```
"knowledgeStore": {
      "storageConnectionString": "<storage_connection_string>",
      "projections": [
        {
            "objects": [
                {
                "storageContainer": "<container>",
                "source": "/projection"
                }
            ],
            "tables": [],
            "files": []
        },
        {
            "objects": [],
            "tables": [
                {
                "tableName": "KeyPhrases",
                "generatedKeyName": "keyphrase_id",
                "source": "projection/key_phrases/*",
                },
                {
                "tableName": "docs",
                "generatedKeyName": "document_id",
                "source": "/projection"
                }
            ],
            "files": []
        },
        {
            "objects": [],
            "tables": [],
            "files": [
                {
                "storageContainer": "<container>",
                "source": "/document/normalized_images/*"
                }
            ]
        }
    ]
 }
```

For **object** and **file** projections, the specified container will be created if it does not already exist. An Azure Storage table will be created for each **table** projection, with the mapped fields and a unique key field with the name specified in the **generatedKeyName** property. These key fields can be used to define relational joins between the tables for analysis and reporting.

# Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/03-knowledge-store.html
