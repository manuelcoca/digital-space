# Triggers & Bindings

Triggers and bindings let you avoid hardcoding access to other services. Depending on the development language, triggers and bindings are defined differently:

* **C#** - Decorating methods and parameters with C# attributes
* **Java** - Decorating methods and parameters with Java annotations
* **JavaScript/PowerShell/Python/TypeScript** - Updating _function.json_ schema

## Function.json

The _function.json_ file defines the function's trigger, bindings, and other configuration settings.&#x20;

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

The `bindings` property is where you configure both triggers and bindings. Each binding shares a few common settings and some settings that are specific to a particular type of binding. Every binding requires the following settings:

<table><thead><tr><th width="163.33333333333331">Property</th><th width="115">Types</th><th>Comments</th></tr></thead><tbody><tr><td><code>type</code></td><td>string</td><td>Name of binding. For example, <code>queueTrigger</code>.</td></tr><tr><td><code>direction</code></td><td>string</td><td>Indicates whether the binding is for receiving data into the function or sending data from the function. For example, <code>in</code> or <code>out</code>.</td></tr><tr><td><code>name</code></td><td>string</td><td>The name that is used for the bound data in the function. For example, <code>myQueue</code>.</td></tr></tbody></table>

## Triggers

A trigger defines how a function is invoked and a function must have exactly one trigger. Triggers have associated data, which is often provided as the payload of the function.

## Bindings

Binding to a function is a way of connecting another resource to the function; bindings may be connected as _input bindings_, _output bindings_, or both. Data from bindings is provided to the function as parameters.

### Azure Blob Output Binding

With an Blob Output Binding it is possible to store an file passed via API into a Blob Storage very easy.

1. Navigate to Functions > Integrations > Output&#x20;
2. Create new Output Binding
   1. Binding Type - Set to Blob Storage
   2. Storage Account - Select or create a new Storage Account
   3. Path \<container-name>/\<file-name> - Azure tries to safe the files into Container. So a new Container with the same name needs to be created in the Storage Account
3. Navigate to the Storage Account > Containers and create a new Container to store files

## Implementation Examples

Suppose you want to write a new row to Azure Table storage whenever a new message appears in Azure Queue storage. This scenario can be implemented using an Azure Queue storage trigger and an Azure Table storage output binding.

### Function.json implementation

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

### C# implementation

```csharp
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

### JavaScript implementation

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
module.exports = async function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.bindings.order = order;
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```
