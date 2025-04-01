---
tags:
    - azureai
    - azure
---

## Authentication

By default, access to Azure AI services resources is restricted by using subscription keys. Management of access to these keys is a primary consideration for security.

### Subscription/Account keys

-   Keys should be regenerated regularly
-   Keys can be regenerated via Azure Portal or Azure CLI

```
// Regenerate keys
$ az cognitiveservices account keys regenerate

// List account keys
$ az cognitiveservices account keys list --name --resource-group
```

Each service has always 2 keys which allows key regeneration without service interruption:

-   Configure production application to only use 1 key
-   Regenerate key 2
-   Change application configs to use key 2 in all production application
-   Regenerate key 1
-   Change app configs to use key 1 again

### Protect Keys with KeyVault

Keys can be stored securely in Azure KeyVault and applications can retrieve the keys from key vault by assigning a managed identity to the applications. Those managed identities area authenticated via Microsoft Entra ID. This solution minimize the risk of keys being compromised by hard-coded keys in code or config files.

```
// Create security principal
$ az ad sp create-for-rbac -n "api://<spName>" --role owner --scopes subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>

// Set Policy for key vault
$ az ad sp show --id <appId> --query objectId --out tsv
$ az keyvault set-policy -n <keyVaultName> --object-id <objectId> --secret-permission
```

Then in code use following informations for authentication:

-   The **endpoint** for your Azure AI Services resource
-   The name of your **Azure Key Vault** resource
-   The **tenant** for your service principal
-   The **appId** for your service principal
-   The **password** for your service principal

### Token-based authentication

Some Azure AI Services support (or even *require*) token-based authentication. In these cases, the subscription key is presented in an initial request to obtain an authentication token, which has a valid period of 10 minutes. When using an SDK, the SDK will handle the token authentication automatically.

## Network access restrictions

Some Azure AI Services can be configured to restrict access to specific network addresses. With network restrictions enabled, a client trying to connect from an IP address that is not allowed will receive an **Access Denied** error.
