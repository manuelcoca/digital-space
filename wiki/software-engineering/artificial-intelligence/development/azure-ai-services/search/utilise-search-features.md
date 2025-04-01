---
tags: azureai, azure, azureaisearch
---

After data index was created and populated ([read here](https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/01-azure-search.html)), you can query via REST API or Search Explorer Tool on Azure Portal to search for informations in the indexed data.

Search works best when most relevant result are shown first and the user gets exactly what he is looking for without writing complex search terms. In order to to improve search results there are several techniques and search features available:

## Simple text search

Azure Cognitive Search implements enhanced version of **Apache Lucene** and therefore uses **Lucene query syntax**, which provides a rich set of query operations for searching, filtering, and sorting data in indexes. Azure Cognitive Search supports two variants of the **Lucene syntax**:

-   **Simple** - An intuitive syntax that makes it easy to perform basic searches that match literal query terms submitted by a user.
-   **Full** - An extended syntax that supports complex filtering, regular expressions, and other more sophisticated queries.

Common search parameters supported by Lucene that can be submitted with a query:

-   **search** - A search expression that includes the terms to be found.
-   **queryType** - The Lucene syntax to be evaluated (*simple* or *full*).
-   **searchFields** - The index fields to be searched.
-   **select** - The fields to be included in the results.
-   **searchMode** - Criteria for including results based on multiple search terms. For example, suppose you search for *comfortable hotel*. A searchMode value of *Any* returns documents that contain "comfortable", "hotel", or both; while a searchMode value of *All* restricts results to documents that contain both "comfortable" and "hotel".

Example query: Search for luxury hotels and select only specific fields. The query will search for the term `luxury` across all fields.

```
search=luxury&$select=HotelId, HotelName, Category, Tags, Description&$count=true
```

## Full text serach

To enable full text search add `&queryType=full` to the query string. Full text search allows advanced search features:

-   **Boolean operators**: `AND`, `OR`, `NOT` for example `luxury AND 'air con'`
-   **Fielded search**: `fieldName:search term` for example `Description:luxury AND Tags:air con`
-   **Fuzzy search**: `~` for example `Description:luxury~` returns results with misspelled versions of luxury
-   **Term proximity search**: `"term1 term2"~n` for example `"indoor swimming pool"~3` returns documents with the words indoor swimming pool within three words of each other
-   **Regular expression search**: `/regular expression/` use a regular expression between `/` for example `/[mh]otel/`would return documents with hotel and motel
-   **Wildcard search**: `*`, `?` where `*` will match many characters and `?` matches a single character for example `'air con'*` would find air con and air conditioning
-   **Precedence grouping**: `(term AND (term OR term))` for example `(Description:luxury OR Category:luxury) AND Tags:air?con*`
-   **Term boosting**: `^` for example `Description:luxury OR Category:luxury^3` would give hotels with the category luxury a higher score than luxury in the description

Example: Compared with the query from Simple text search (read above), this query gives higher priority to hotels in the luxury category. It also looks for air conditioning in the Tags field:

```
search=(Description:luxury OR Category:luxury^3) AND Tags:'air con'*&$select=HotelId, HotelName, Category, Tags, Description&$count=true&queryType=full
```

## Filtering

You can apply filters to queries in two ways:

-   By including filter criteria in a *simple* **search** expression.
-   By providing an OData filter expression as a **$filter** parameter with a *full* syntax search expression.

You can apply a filter to any *filterable* field in the index.

Example: Find documents containing the text *London* that have an **author** field value of *Reviewer*.

```
search=London+author='Reviewer'
queryType=Simple
```

Or use OData filter in a **$filter** parameter with a *full* Lucene search expression like this:

```
search=London
$filter=author eq 'Reviewer'
queryType=Full
```

## Sorting

By default, results are sorted based on the relevancy score assigned by the query process, with the highest scoring matches listed first. However, you can override this sort order by including an OData **orderby** parameter that specifies one or more *sortable* fields and a sort order (*asc* or *desc*).

Example _desc_ sorting:

```
search=*
$orderby=last_modified desc
```

## Search-as-you-type

By adding a *suggester* to an index, you can enable two forms of search-as-you-type experience to help users find relevant results more easily:

-   *Suggestions* - retrieve and display a list of suggested results as the user types into the search box, without needing to submit the search query.
-   *Autocomplete* - complete partially typed search terms based on values in index fields.

To implement one or both of these capabilities, create or update an index, defining a suggester for one or more fields.

After you've added a suggester, you can use the **suggestion** and **autocomplete** REST API endpoints or the .NET **DocumentsOperationsExtensions.Suggest** and **DocumentsOperationsExtensions.Autocomplete** methods to submit a partial search term and retrieve a list of suggested results or autocompleted terms to display in the user interface.

## Synonyms

Often, the same thing can be referred to in multiple ways. For example, someone searching for information about the United Kingdom might use any of the following terms:

-   United Kingdom
-   UK
-   Great Britain\*
-   GB\*

To help users find the information they need, you can define *synonym maps* that link related terms together. You can then apply those synonym maps to individual fields in an index, so that when a user searches for a particular term, documents with fields that contain the term or any of its synonyms will be included in the results.

## Scoring Profiles

Azure Cognitive Search uses the BM25 similarity ranking algorithm which scores the documents returned from the first three phases. The score is a function of the number of times identified search terms appear in a document, the document's size, and the rarity of each of the terms.

Sometimes the default scoring is not sufficient and Cognitive Search lets you influence a document score using scoring profiles. Scoring profiles defines weights for fields.

![[weighted-field-score.png]]

In the above example, the description field has more weight then any other field. Instead of simple weights, the scoring profiles can also include functions for more advanced scoring logic.

### Add a weighted scoring profile

You can add up to 100 scoring profiles to a search index. The simplest way to create a scoring profile is in the Azure portal.

1. Navigate to your search service.
2. Select **Indexes**, then select the index to add a scoring profile to.
3. Select **Scoring profiles**.
4. Select **+ Add scoring profile**.
5. In **Profile name**, enter a unique name.
6. To set the scoring profile as a default to be applied to all searches select **Set as default profile**.
7. In **Field name**, select a field. Then for **Weight**, enter a weight value.
8. Select **Save**.

![[azure-portal-scoring-profiles.png]] In the above example, the `boost-category` scoring profile has been added to the `hotels-sample-index`. The Category has a weight of five.

The profile has also been set as the default profile. You can then use this search query:

`search=luxury AND Tags:'air con'*&$select=HotelId, HotelName, Category, Tags, Description&$count=true&queryType=full`

The results now match the same query with a term boosted:

`search=(Description:luxury OR Category:luxury^5) AND Tags:'air con'*&$select=HotelId, HotelName, Category, Tags, Description&$count=true&queryType=full`

You can control which scoring profile is applied to a search query by appending the `&scoringProfile=PROFILE NAME`parameter.

Scoring profiles can also be added programmatically using the Update Index REST API or in Azure SDKs, such as the ScoringProfile class in the Azure SDK for .NET.

### Use functions in a scoring profile

The functions available to add to a scoring profile are:

| Function  | Description                                                                                |
| --------- | ------------------------------------------------------------------------------------------ |
| Magnitude | Alter scores based on a range of values for a numeric field                                |
| Freshness | Alter scores based on the freshness of documents as given by a DateTimeOffset field        |
| Distance  | Alter scores based on the distance between a reference location and a GeographyPoint field |
| Tag       | Alter scores based on common tag values in documents and queries                           |

For example, using the hotel index the magnitude function can be applied to the Rating field. The Azure portal will guide you through completing the parameters for each function.![[function-parameters.png]]

## Analysers and tokenised terms

When Cognitive Search indexes your content, it retrieves text. To build a useful index, with terms that help users locate documents, that text needs processing. For example:

-   The text should be broken into words, often by using whitespace and punctuation characters as delimiters.
-   Stopwords, such as "the" and "it", should be removed because users don't search for them.
-   Words should be reduced to their root form. For example, past tense words, such as "ran", should be replaced with present tense words, such as "run".

In Cognitive Search, this kind of processing is performed by analyzers. If you don't specify an analyzer for a field, the default Lucene analyzer is used. The default Lucene analyzer is a good choice for most fields because it can process many languages and return useful tokens for your index.

Alternatively, you can specify one of the analyzers that are built into Cognitive Search. Built-in analyzers are of two types:

-   **Language analyzers.** If you need advanced capabilities for specific languages, such as lemmatization, word decompounding, and entity recognition, use a built-in language analyzer. Microsoft provides 50 analyzers for different languages.
-   **Specialized analyzers.** These analyzers are language-agnostic and used for specialized fields such as zip codes or product IDs. You can, for example, use the **PatternAnalyzer** and specify a regular expression to match token separators.

### Custom Analyzer

The built-in analyzers provide you with many options but sometimes you need an analyzer with unusual behavior for a field. In these cases, you can create a **custom analyzer**.

A custom analyzer consists of:

-   **Character filters**. These filters process a string before it reaches the tokenizer.
    -   **html_strip.** This filter removes HTML constructs such as tags and attributes.
    -   **mapping.** This filter enables you to specify mappings that replace one string with another. For example, you could specify a mapping that replaces *TX* with *Texas*.
    -   **pattern_replace.** This filter enables you to specify a regular expression that identifies patterns in the input text and how matching text should be replaced.
-   **Tokenizers**. These components divide the text into tokens to be added to the index.
    -   **classic.** This tokenizer processes text based on grammar for European languages.
    -   **keyword.** This tokenizer emits the entire input as a single token. Use this tokenizer for fields that should always be indexed as one value.
    -   **lowercase.** This tokenizer divides text at non-letters and then modifies the resulting tokens to all lower case.
    -   **microsoft_language_tokenizer.** This tokenizer divides text based on the grammar of the language you specify.
    -   **pattern.** This tokenizer divides texts where it matches a regular expression that you specify.
    -   **whitespace.** This tokenizer divides text wherever there's white space.
-   **Token filters.** These filters remove or modify the tokens emitted by the tokenizer.
    -   Language-specific filters, such as **arabic_normalization**. These filters apply language-specific grammar rules to ensure that forms of words are removed and replaced with roots.
    -   **apostrophe**. This filter removes any apostrophe from a token and any characters after the apostrophe.
    -   **classic.** This filter removes English possessives and dots from acronyms.
    -   **keep.** This filter removes any token that doesn't include one or more words from a list you specify.
    -   **length.** This filter removes any token that is longer than your specified minimum or shorter than your specified maximum.
    -   **trim.** This filter removes any leading and trailing white space from tokens.

### Create custom Analyzer

You create a custom analyzer by specifying it when you define the index. You must do this with JSON code.

In this example, a character filter removes HTML formatting, a tokenizer splits the text according to the grammar of Icelandic, and a token filter removes apostrophes:

```
"analyzers":(optional)[
   {
      "name":"ContosoAnalyzer",
      "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
      "charFilters":[
         "WebContentRemover"
      ],
      "tokenizer":"IcelandicTokenizer",
      "tokenFilters":[
         "ApostropheFilter"
      ]
   }
],
"charFilters":(optional)[
   {
      "name":"WebContentRemover",
      "@odata.type":"#html_strip"
   }
],
"tokenizers":(optional)[
   {
      "name":"IcelandicTokenizer",
      "@odata.type":"#microsoft_language_tokenizer",
      "language":"icelandic",
      "isSearchTokenizer":false,
   }
],
"tokenFilters":(optional)[
   {
      "name":"ApostropheFilter",
      "@odata.type":"#apostrophe"
   }
]
```

### Use a custom Analyzer

Once you've defined and tested a custom analyzer, you can configure your index to use it. You can specify an analyzer for each field in your index.

You can use the `analyzer` field when you want to use the same analyzer for both indexing and searching:

```
"fields": [
 {
   "name": "IcelandicDescription",
   "type": "Edm.String",
   "retrievable": true,
   "searchable": true,
   "analyzer": "ContosoAnalyzer",
   "indexAnalyzer": null,
   "searchAnalyzer": null
 },
```

## Index Multi-Language Support

To add multiple languages to an index, first, identify all the fields that need a translation. Then duplicate those fields for each language you want to support.

For example, if an index has an English description field, you'd add description_fr for the French translation and description_de for German. For each field, add to its definition the [corresponding language analyzer](https://learn.microsoft.com/en-us/azure/search/index-add-language-analyzers#supported-language-analyzers).

The JSON definition of the index could look like this:

```
{
      "name": "description",
      "type": "Edm.String",
      "facetable": false,
      "filterable": false,
      "key": false,
      "retrievable": true,
      "searchable": true,
      "sortable": false,
      "analyzer": "en.microsoft",
      "indexAnalyzer": null,
      "searchAnalyzer": null,
      "synonymMaps": [],
      "fields": []
    },
    {
      "name": "description_de",
      "type": "Edm.String",
      "facetable": false,
      "filterable": false,
      "key": false,
      "retrievable": true,
      "searchable": true,
      "sortable": false,
      "analyzer": "de.microsoft",
      "indexAnalyzer": null,
      "searchAnalyzer": null,
      "synonymMaps": [],
      "fields": []
    },
    {
      "name": "description_fr",
      "type": "Edm.String",
      "facetable": false,
      "filterable": false,
      "key": false,
      "retrievable": true,
      "searchable": true,
      "sortable": false,
      "analyzer": "fr.microsoft",
      "indexAnalyzer": null,
      "searchAnalyzer": null,
      "synonymMaps": [],
      "fields": []
    },
```

### Add translation skillset

If you don't have access to translations, you can enrich your index and add translated fields using Azure AI.

The steps are to add fields for each language, add a skill for each language, and then map the translated text to the correct fields.

## Geo-Distance Search

To help you compare locations on the Earth's surface, Cognitive Search includes geo-spatial functions that you can call in queries.

To ask Cognitive Search to return results based on their location information, you can use two functions in your query:

-   `geo.distance`. This function returns the distance in a straight line across the Earth's surface from the point you specify to the location of the search result.
-   `geo.intersects`. This function returns `true` if the location of a search result is inside a polygon that you specify.

To use these functions, make sure that your index includes the location for results. Location fields should have the datatype `Edm.GeographyPoint` and store the latitude and longitude.

### Use geo.distance function

`geo.distance` is a function that takes two points as parameters and returns the distance between them in kilometers. Example filter:

```
search=(Description:luxury OR Category:luxury)$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5&$select=HotelId, HotelName, Category, Tags, Description&$count=true
```

Example for filtering hotels sorted by closest with `orderBy`:

```
search=(Description:luxury OR Category:luxury)&orderby=geo.distance(Location, geography'POINT(2.294481 48.858370)') asc&$select=HotelId, HotelName, Category, Tags, Description&$count=true
```

## Semantic Search

Semantic search is a capability of Azure Cognitive Search and has two functions; it improves the ranking of the query results based on language understanding and it improves the response to the query by providing captions and answers in the results.

### How semantic search works

Semantic ranking takes the top 50 results from the BM25 ranking results. The results are split into multiple fields as defined by a semantic configuration. The fields are converted into text strings and trimmed to 256 unique tokens.

Once the strings are prepared, they are passed to machine reading comprehension models to find the phrases and sentences that best match the query. The results of this summarization phrase is a semantic caption and, optionally, a semantic answer.

The semantic captions are now ranked based on the semantic relevance of the caption. The results are then returned in descending order of relevance.

### Semantic search pricing

Up to 1000 semantic search queries a month are available free of charge.

For more than 1000 queries a month, you should choose standard pricing. The cost of standard pricing is based on the volume of searches, the type of searches, and the region of the search.

For more information on semantic search pricing, see [Azure Cognitive Search pricing](https://azure.microsoft.com/pricing/details/search/)

### Set up semantic search

First semantic search need be enabled at service level. Once enabled, it is available for all indexes. Semantic search is also only available in few regions ([see here](https://azure.microsoft.com/explore/global-infrastructure/products-by-region/?products=search)).

Enable Semantic Search:

1. Open the Azure portal and sign in.
2. Select **All resources** and select your search service.
3. In the navigation pane, select **Semantic search (preview)**.
4. Select the appropriate service plan. You can alter the service plan after deployment

After enabling, it can be configured per-index:

1. From the Azure portal home page, select **All resources** and select your search service.
2. On the navigation bar, in **Search management**, select **Indexes**. ![Screenshot of Indexes button.](https://learn.microsoft.com/en-us/training/wwl-data-ai/use-semantic-search/media/3-indexes.png)
3. Select your index.
4. Select **Semantic configurations** and select **Add semantic configuration**.
5. In **Name** type a name for your semantic configuration.
6. In **Title field** select the field that describes the document.
7. Under **Content fields**, in **Field name**, select a content field.
8. Repeat the previous step for additional content fields.
9. Under **Keyword fields**, in **Field name**, select a field with key phrases.
10. Repeat the previous step for additional keyword fields.
11. Select **Save**.
12. On your index page, select **Save**.

## Vector Search

A vector query can be used to match criteria across different types of source data (image, video, audio) by providing a mathematical representation of the content generated by machine learning models. ![[vector-search-architecture-diagram.png]] Here are some scenarios where you should use vector search:

-   Use OpenAI or open source models to encode text, and use queries encoded as vectors to retrieve documents.
-   Do a similarity search across encoded images, text, video and audio, or a mixture of these (multi-modal).
-   Represent documents in different languages using a multi-lingual embedded model to find documents in any language.
-   Build hybrid searched from vector and searchable text fields as vector searches are implemented at field level. The results will be merged to return a single response.
-   Apply filters to text and numeric fields and include this in your query to reduce the data that your vector search needs to process.
-   To create a vector database to provide an external knowledge base or use as a long term memory.

### Limitations

-   You'll need to provide the embeddings using Azure OpenAI or a similar open source solution, as Azure Cognitive Search doesn't generate these for your content.
-   Customer Managed Keys (CMK) aren't supported.
-   There are storage limitations applicable so you should check what your service quota provides.

### How search works

-   Index need to have a vector field for storing the embeddings
-   Query input need to be converted into a vector by using the embedding library used to create the source document embeddings

## Resources

-   Training Resource: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/05-exercise-implement-enhancements-to-search-results.html
-   Semantic Search Training: https://microsoftlearning.github.io/mslearn-knowledge-mining/Instructions/Labs/09-semantic-search-exercise.html
-   Vector Search Training: https://learn.microsoft.com/en-us/training/modules/improve-search-results-vector-search/5-exercise
