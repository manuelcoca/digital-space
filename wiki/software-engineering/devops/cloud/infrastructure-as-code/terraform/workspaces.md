---
tags:
    - terraform
---

Workspaces helps to manage multiple environments in terraform like dev, staging and prod.

# Commands

List all workspaces:
`$ terraform workspace list`

Create new workspace _dev_ and _prod_:
`$ terraform workspace new dev`

`$ terraform workspace new prod`

Switch between workspaces:
`$ terraform workspace select dev`

# Usage workspace specific configs

In the below example, the tag name would be "example-server-dev" or "example-server-prod" depending on how the workspace is called:

```
tags = {
	Name = "example-server-${terraform.workspace}"
}
```

Terraform also allows conditions to check for the workspace:

```
terraform {
  backend "s3" {
    // THIS CODE WILL NOT WORK!!!!
    bucket = (
      terraform.workspace == "prod"
      ? "prod-bucket"
      : "example-bucket"
    )
    // THIS CODE WILL NOT WORK!!!!    region = "us-east-2"
    key    = "example/terraform.tfstate"
  }
}
```
