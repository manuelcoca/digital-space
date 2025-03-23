Azure Key Vault allows to store secrets, keys and certificates in a secure environment. The main idea is:

-   Centralized and secured environment for storing secrets, keys and certificates
-   Only accessable by few users
-   Monitor access and use
-   Simplified administration

So It provides enhanced security but comes with more difficulty compared to storing Secrets in app configurations.

## Authentication <a href="#authentication" id="authentication"></a>

To do any operations with Key Vault, you first need to authenticate to it. There are three ways to authenticate to Key Vault:

-   **Managed identities for Azure resources**: When you deploy an app on a virtual machine in Azure, you can assign an identity to your virtual machine that has access to Key Vault. You can also assign identities to other Azure resources. The benefit of this approach is that the app or service isn't managing the rotation of the first secret. Azure automatically rotates the service principal client secret associated with the identity. We recommend this approach as a best practice.
-   **Service principal and certificate**: You can use a service principal and an associated certificate that has access to Key Vault. We don't recommend this approach because the application owner or developer must rotate the certificate.
-   **Service principal and secret**: Although you can use a service principal and a secret to authenticate to Key Vault, we don't recommend it. It's hard to automatically rotate the bootstrap secret that's used to authenticate to Key Vault.

## Encryption of data in transit <a href="#encryption-of-data-in-transit" id="encryption-of-data-in-transit"></a>

Azure Key Vault enforces Transport Layer Security (TLS) protocol to protect data when it’s traveling between Azure Key Vault and clients. Clients negotiate a TLS connection with Azure Key Vault. TLS provides strong authentication, message privacy, and integrity (enabling detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, and ease of deployment and use.

Perfect Forward Secrecy (PFS) protects connections between customers’ client systems and Microsoft cloud services by unique keys. Connections also use RSA-based 2,048-bit encryption key lengths. This combination makes it difficult for someone to intercept and access data that is in transit.

## Creating Azure Key Vault

{% tabs %} {% tab title="Basics" %}

1. **`Pricing tier`**
    - Standard (Software generated random key)
    - Premium (Hardware generated random key)
2. **`Soft delete`**
    1. Secrets or Key Vault will not be deleted completely. The user has time to recover for the set amount of days. {% endtab %}

{% tab title="Access configuration" %} To access a key vault, all callers (users or applications) must have proper authentication and authorization.

1. **`Permission model`**
    - Azure RBAC (recommended) - Using Azure Roles to controll access (fine-grained)
    - Vault access policy - Configure user based policies to controll access {% endtab %}

{% tab title="Networking" %} This configuration allows to restrict access to the Key Vault by network. Networking settings similar to other resources (see [App Service](../serverless/app-service/) or [Function App](../serverless/function-app/)). {% endtab %} {% endtabs %}

## Menu/Configuration

<details>

<summary>Access policies</summary>

Configure policies to grant access to the Key Vault. Only configurable if **`Permission model`** is set to **`Vault access policy`**.

</details>

## Azure Key Vault best practices <a href="#azure-key-vault-best-practices" id="azure-key-vault-best-practices"></a>

-   **Use separate key vaults:** Recommended using a vault per application per environment (Development, Pre-Production and Production). This pattern helps you not share secrets across environments and also reduces the threat if there is a breach.
-   **Control access to your vault:** Key Vault data is sensitive and business critical, you need to secure access to your key vaults by allowing only authorized applications and users.
-   **Backup:** Create regular back ups of your vault on update/delete/create of objects within a Vault.
-   **Logging:** Be sure to turn on logging and alerts.
-   **Recovery options:** Turn on [soft-delete](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview) and purge protection if you want to guard against force deletion of the secret.

## Work with Azure CLI

### Create a KeyVault

```
myKeyVault=az204vault-$RANDOM
myLocation=<myLocation>

$ az group create --name az204-vault-rg --location $myLocation
$ az keyvault create --name $myKeyVault --resource-group az204-vault-rg --location $myLocation

```

### Add an retrieve secrets

<pre><code><strong>$ az keyvault secret set --vault-name $myKeyVault --name "ExamplePassword" --value "hVFkk965BuUv"
</strong><strong>$ az keyvault secret show --name "ExamplePassword" --vault-name $myKeyVault
</strong></code></pre>
