---
tags: terraform
---

Each Resource block defines one or more infrastructure object (e.g Storage Account, Virtual Network, Web App etc.).

# Resource Structure

A `aws_instance` block declares a resource of a specific type with a specific local name `web`:

```
resource "aws_instance" "web" {
  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"
}
```

-   The resource name is used to refer to that resource within the same Terraform module - no meaning outside of module's scope.

# Resource Arguments

Most of arguments within the body of a `resource` are specf

https://developer.hashicorp.com/terraform/language/resources/syntax
