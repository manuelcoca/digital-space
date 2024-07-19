# Lifecycle Management

Data sets have unique lifecycles. Some data expires days or months after creation, while other data sets are actively read and modified throughout their lifetimes.

Azure storage offers different access tiers, allowing you to store blob object data in the most cost-effective manner. Available access tiers include:

* **Hot**&#x20;
  * Optimized for storing data that is accessed frequently
  * Highest storage costs but lowest access costs
  * New Storage accounts are created in hot tier by default
* **Cool**&#x20;
  * Optimized for storing data that is infrequently accessed and stored for at least 30 days
  * Lower storage costs but higher access costs compared to Hot tier.
* **Archive**
  * Available only for individual block blobs
  * Optimized for data that can tolerate several hours of retrieval latency and will remain in the Archive tier for at least 180 days
  * Most cost-effective option for storing data, but high costs for accessing data&#x20;

### Considerations

* The access tier can be set on a blob during or after upload
* Only the hot and cool access tiers can be set at the account level. The archive access tier can only be set at the blob level
* Data in the cool access tier has slightly lower availability, but still has high durability, retrieval latency, and throughput characteristics similar to hot data
* The hot and cool tiers support all redundancy options. The archive tier supports only LRS, GRS, and RA-GRS.

## Lifecycle policies

A policy is a collection of rules in a JSON format.

### JSON structure

```json
{
  "rules": [
    {
      "name": "rule1",        # Rule name
      "enabled": true,        # Activate/Deactivate rule
      "type": "Lifecycle",    # Only available type is "Lifecycle"
      "definition": {...}     # Rule definition
    },
    ...
  ]
}
```

### Rules

Rules are defined inside the **`definition`** object. Each definition object includes a filter set and action set

* Filter set limits rule actions to certain set of objects within a container
* Action set applies the tier or delete actions to the filtered objects

The following sample rule filters the account to run the actions on objects that exist inside `container1` and start with `foo`.

* Tier blob to cool tier 30 days after last modification
* Tier blob to archive tier 90 days after last modification
* Delete blob 2,555 days (seven years) after last modification
* Delete blob snapshots 90 days after snapshot creation

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

## Create rules

Rules can be created with:

* Azure portal
* Azure PowerShell
* Azure CLI
* REST APIs

### Azure CLI

```
az storage account management-policy create \
    --account-name <storage-account> \
    --policy @policy.json \
    --resource-group <resource-group>

```

## Rehydrate blob data from the archive tier

A blob in the archive access tier is offline and can't be read or modified. In order to read or modify data in an archived blob, you must first rehydrate the blob to an online tier, either the hot or cool tier. There are two options for rehydrating a blob that is stored in the archive tier:

* **Copy an archived blob to an online tier**: You can rehydrate an archived blob by copying it to a new blob in the hot or cool tier with the [Copy Blob](https://learn.microsoft.com/en-us/rest/api/storageservices/copy-blob) or [Copy Blob from URL](https://learn.microsoft.com/en-us/rest/api/storageservices/copy-blob-from-url) operation. Microsoft recommends this option for most scenarios.
* **Change a blob's access tier to an online tier**: You can rehydrate an archived blob to hot or cool by changing its tier using the [Set Blob Tier](https://learn.microsoft.com/en-us/rest/api/storageservices/set-blob-tier) operation.

### Rehydration priority

When you rehydrate a blob, you can set the priority for the rehydration operation via the optional `x-ms-rehydrate-priority` header on a [Set Blob Tier](https://learn.microsoft.com/en-us/rest/api/storageservices/set-blob-tier) or **Copy Blob/Copy Blob From URL** operation. Rehydration priority options include:

* **Standard priority**: The rehydration request is processed in the order it was received and may take up to 15 hours.
* **High priority**: The rehydration request is prioritized over standard priority requests and may complete in under one hour for objects under 10 GB in size.
