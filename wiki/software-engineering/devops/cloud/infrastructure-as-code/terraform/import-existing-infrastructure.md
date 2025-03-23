---
tags:
    - terraform
---

Terraform is designed to allow you to manage only a subset of your infrastructure with a particular Terraform configuration, in case either some objects are managed by another tool or in case you've decomposed your infrastructure to be managed by many separate configurations that cooperate to produce the desired result.

Another scenario is that many developers first manually create cloud resources and test around. After resources are created manually, they can be imported and managed by terraform.

So Terraform makes a distinction between an object existing in the remote system and that object being managed by the current Terraform configuration.

Example of importing a _azurerm_ _resource-group_:

```
terraform import azurerm_resource_group.<resourcename>"/subscriptions/<subscription.id>/resourceGroups/<resource-group-name>"
```

Example of importing an _azurerm keyvault module_:

```
terraform import module.keyvault.azurerm_key_vault.<resource-name> "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<keyvault-name>"
```
