---
tags: azure, azureai, azureaisearch
---

Create **Azure Cognitive Search** resource in Azure Portal.

# Service tiers and capacity management

Available pricing tiers:

| Pricing Tier      | Description                                                                                                                                                                                                                                           |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Free (F)          | Explore and learn                                                                                                                                                                                                                                     |
| Basic (B)         | Small-scale search solutions for max. 15 indexes and 2GB of data                                                                                                                                                                                      |
| Standard (S)      | Enterprise-scale solutions. There are multiple variants of this tier, including S, S2, and S3; which offer increasing capacity in terms of indexes and storage, and S3HD, which is optimized for fast read performance on smaller numbers of indexes. |
| Storage Optimized | Use a storage optimized tier (L1 or L2) when you need to create large indexes, at the cost of higher query latency.                                                                                                                                   |

![[cognitive-search-pricing-tier.png]]

# Replicas and partitions

Depending on the pricing tier you select, you can optimise your solution for scalability and availability by creating replicas and partitions.

-   *Replicas* are instances of the search service - Increasing the number of replicas can help ensure there is sufficient capacity to service multiple concurrent query requests while managing ongoing indexing operations.
-   *Partitions* are used to divide an index into multiple storage locations, enabling you to split I/O operations such as querying or rebuilding an index.

The combination of replicas and partitions you configure determines the *search units* used by your solution. Put simply, the number of search units is the number of replicas multiplied by the number of partitions (R x P = SU). For example, a resource with four replicas and three partitions is using 12 search units.

# Components of Cognitive Search Solution

## Data Source

Data source that contains the data you want to search. Following data source types are supported:

-   Azure Blob Storage Containers - For unstructured files
-   Azure SQL Database
-   CosmosDB

Or applications can push JSON data directly into an index.

## Skillset

In Basic search solutions, when indexing data from a database, fields in database tables are extracted, and when indexing documents, file metadata (e.g., name, date, size, author) and text content are extracted.

Modern applications require richer insights into data. Azure Cognitive Search can use AI skills during indexing to enrich source data, which can be mapped to index fields.

Examples of AI skills included:

-   The language in which a document is written.
-   Key phrases that might help determine the main themes or topics discussed in a document.
-   A sentiment score that quantifies how positive or negative a document is.
-   Specific locations, people, organizations, or landmarks mentioned in the content.
-   AI-generated descriptions of images, or image text extracted by optical character recognition.
-   Custom skills that you develop to meet specific requirements.

## Indexer

-   The indexer is responsible for driving the indexing process.
-   It takes extracted outputs from the skillset, as well as data and metadata values from the data source, and maps them to fields in the index.
-   An indexer is automatically executed upon creation and can be scheduled to run at regular intervals or run on demand to add more documents to the index.
-   Resetting the index may be necessary when adding new fields to the index or introducing new skills to the skillset before re-running the indexer.

## Index

The index is the searchable result of the indexing process. It consists of a collection of JSON documents, with fields that contain the values extracted during indexing. Client applications can query the index to retrieve, filter, and sort information.

Each index field can be configured with the following attributes:

-   **key**: Fields that define a unique key for index records.
-   **searchable**: Fields that can be queried using full-text search.
-   **filterable**: Fields that can be included in filter expressions to return only documents that match specified constraints.
-   **sortable**: Fields that can be used to order the results.
-   **facetable**: Fields that can be used to determine values for *facets* (user interface elements used to filter the results based on a list of known field values).
-   **retrievable**: Fields that can be included in search results (_by default, all fields are retrievable unless this attribute is explicitly removed_).

# Understand the indexing process

The indexing process works by creating a **document** for each indexed entity. During indexing, an enrichment pipeline_iteratively builds the documents that combine metadata from the data source with enriched fields extracted by cognitive skills. You can think of each indexed document as a JSON structure, which initially consists of a **document** with the index fields you have mapped to fields extracted directly from the source data, like this:

-   **document**
    -   **metadata_storage_name**
    -   **metadata_author**
    -   **content**

When the documents in the data source contain images, you can configure the indexer to extract the image data and place each image in a **normalized_images** collection, like this:

-   **document**
    -   **metadata_storage_name**
    -   **metadata_author**
    -   **content**
    -   **normalized_images**
        -   **image0**
        -   **image1**

Normalizing the image data in this way enables you to use the collection of images as an input for skills that extract information from image data.

Each skill adds fields to the **document**, so for example a skill that detects the _language_in which a document is written might store its output in a **language** field, like this:

-   **document**
    -   **metadata_storage_name**
    -   **metadata_author**
    -   **content**
    -   **normalized_images**
        -   **image0**
        -   **image1**
    -   **language**

The document is structured hierarchically, and the skills are applied to a specific context within the hierarchy, enabling you to run the skill for each item at a particular level of the document. For example, you could run an optical character recognition (_OCR_) skill for each image in the normalized images collection to extract any text they contain:

-   **document**
    -   **metadata_storage_name**
    -   **metadata_author**
    -   **content**
    -   **normalized_images**
        -   **image0**
            -   **Text**
        -   **image1**
            -   **Text**
    -   **language**

The output fields from each skill can be used as inputs for other skills later in the pipeline, which in turn store *their* outputs in the document structure. For example, we could use a *merge* skill to combine the original text content with the text extracted from each image to create a new **merged_content** field that contains all of the text in the document, including image text.

-   **document**
    -   **metadata_storage_name**
    -   **metadata_author**
    -   **content**
    -   **normalized_images**
        -   **image0**
            -   **Text**
        -   **image1**
            -   **Text**
    -   **language**
    -   **merged_content**

The fields in the final document structure at the end of the pipeline are mapped to index fields by the indexer in one of two ways:

1. Fields extracted directly from the source data are all mapped to index fields. These mappings can be *implicit* (fields are automatically mapped to in fields with the same name in the index) or *explicit* (a mapping is defined to match a source field to an index field, often to rename the field to something more useful or to apply a function to the data value as it is mapped).
2. Output fields from the skills in the skillset are explicitly mapped from their hierarchical location in the output to the target field in the index.

# How Query processing works

Processing a search query consists of four stages:

1. _Query parsing_. The search expression is evaluated and reconstructed as a tree of appropriate subqueries. Subqueries might include *term queries* (finding specific individual words in the search expression - for example *hotel*), *phrase queries* (finding multi-term phrases specified in quotation marks in the search expression - for example, *"free parking"*), and *prefix queries* (finding terms with a specified prefix - for example \*air\**, which would match *airway*, *air-conditioning*, and *airport\*).
2. *Lexical analysis* - The query terms are analyzed and refined based on linguistic rules. For example, text is converted to lower case and nonessential *stopwords* (such as "the", "a", "is", and so on) are removed. Then words are converted to their *root* form (for example, "comfortable" might be simplified to "comfort") and composite words are split into their constituent terms.
3. *Document retrieval* - The query terms are matched against the indexed terms, and the set of matching documents is identified.
4. *Scoring* - A relevance score is assigned to each result based on a term frequency/inverse document frequency (TF/IDF) calculation.

# Resources

-   Create Cognitive Search Service and index data: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/01-azure-search.html
