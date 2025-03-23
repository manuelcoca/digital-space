---
tags: azure, azureauth, azure managed identities
---

While developers can securely store the secrets in [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/overview), services need a way to access Azure Key Vault. Managed identities provide an automatically managed identity in Azure Active Directory (Azure AD) for applications to use when connecting to resources that support Azure AD authentication. Applications can use managed identities to obtain Azure AD tokens without having to manage any credentials.

Benefits of managed identity:

-   You don't need to manage credentials. Credentials aren’t even accessible to you.
-   You can use managed identities to authenticate to any resource that supports [Azure AD authentication](https://learn.microsoft.com/en-us/azure/active-directory/authentication/overview-authentication), including your own applications.
-   Managed identities can be used at no extra cost.

# Managed identities types

For both types a Service Principal will be created under the hood.

1. System-assigned managed identity
2. User-assigned managed identity

## System-assigned

Create a managed identity per Azure Resource (e.g Virtual Machine or App Service). That means:

-   Managed identity is tied to the Azure Resource. If the Azure Resource is deleted, the managed identity will be deleted as well
-   Only that Azure resource can use this identity to request tokens from Azure AD
-   You authorize the managed identity to have access to one or more services

## User-assigned

Create a managed identity as a standalone Azure Resource. That means:

-   A user-assigned managed identity can be used by one or multiple Azure Resources
-   You authorize the managed identity to have access to one or more services

# When to use managed identities

![[managed-identities-use-case.png]]
