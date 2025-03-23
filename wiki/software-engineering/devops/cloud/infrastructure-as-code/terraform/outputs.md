---
tags: terraform
---

Terraform outputs **let you share data between Terraform configurations, and with other tools and automation**. Outputs are also the only way to share data from a child module to your configuration's root module.

Outputs can be declared everywhere but it's recommended to declare them in a separate file called `outputs.tf`.
