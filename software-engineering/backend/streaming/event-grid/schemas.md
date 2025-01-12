# Schemas

The Schema defines the JSON format that are used by all event publishers. Event Grid support to schema types:

* Event schema
* Cloud schema

## Event schema

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

### Properties

<table><thead><tr><th width="177">Property</th><th width="111">Type</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td>topic</td><td>string</td><td>No. If not included, Event Grid stamps onto the event. If included, it must match the Event Grid topic Azure Resource Manager ID exactly.</td><td>Full resource path to the event source. This field isn't writeable. Event Grid provides this value.</td></tr><tr><td>subject</td><td>string</td><td>Yes</td><td>Publisher-defined path to the event subject.</td></tr><tr><td>eventType</td><td>string</td><td>Yes</td><td>One of the registered event types for this event source.</td></tr><tr><td>eventTime</td><td>string</td><td>Yes</td><td>The time the event is generated based on the provider's UTC time.</td></tr><tr><td>id</td><td>string</td><td>Yes</td><td>Unique identifier for the event.</td></tr><tr><td>data</td><td>object</td><td>No</td><td>Event data specific to the resource provider. For custom topics, the event publisher determines the data object.</td></tr><tr><td>dataVersion</td><td>string</td><td>No. If not included, it is stamped with an empty value.</td><td>The schema version of the data object. The publisher defines the schema version.</td></tr><tr><td>metadataVersion</td><td>string</td><td>No. If not included, Event Grid will stamp onto the event. If included, must match the Event Grid Schema <code>metadataVersion</code> exactly (currently, only <code>1</code>).</td><td>The schema version of the event metadata. Event Grid defines the schema of the top-level properties. Event Grid provides this value.</td></tr></tbody></table>

## Cloud schema

In addition to its default event schema, Azure Event Grid natively supports events in the JSON implementation of CloudEvents v1.0 and HTTP protocol binding. CloudEvents is an open specification for describing event data.

```json
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```
