**Takeaways**

-   We can build customisable pipelines for advanced RAG applications
-   Customers can ingest their custom data and pipeline/infrastructure is reuseable
-   The pipeline is basically code, that collects data, loads data, prepares data and store the data e.g in AI Search index
-   There is not business use case to update embedding models. If you need to do, create new embeddings and delete the old ones

**Process of RAG**

-   Index
-   **Index**
    -   Load Data
    -   Split (Chunk)
    -   Embed and Store
-   **Retrieve**
-   **Generate**

**When to not use RAG?**

-   No custom knowledge base
-   General available data
-   No frequent data changes
-   No different data sources

**When to use RAG?**

-   Frequent data change
-   Custom data sources
-   Different data sources (structured, unstructured)

**What are the main reasons to have different projects in the same Azure AI Hub?**

-   Resource sharing
-   Collaboration
-   Centralized management
-   Cost efficiency
-   Consistency
-   Scalability
-   Simplified access control

**What are the main differences between serverless and Managed Compute? Why do we suggest deploying serverless for a RAG implementation?**

-   **Serverless:**
    -   No server management
    -   Automatic scaling
    -   Pay-per-use pricing
    -   Event-driven execution
-   **Managed Compute:**
    -   Managed infrastructure
    -   Manual scaling
    -   Fixed pricing models
    -   Persistent execution
-   **Suggested model for a simple RAG:**
    -   **Serverless** for cost efficiency and scalability.
