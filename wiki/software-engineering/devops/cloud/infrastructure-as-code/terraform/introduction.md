---
tags: terraform
---

Terraform is a Infrastructure as Code (IaC) tool that allow to manage infrastructure with human-readable, configurations files, rather than graphical user interface. Advantages:

-   Terraform can manage infrastructure on multiple cloud platforms.
-   The human-readable, configuration language helps you write infrastructure code quickly.
-   Terraform's state allows you to track resource changes throughout your deployments.
-   You can commit your configurations to version control to safely collaborate, reuse and share infrastructure.

Terraform's configuration language is declarative, meaning that it describes the desired end-state for your infrastructure, in contrast to procedural programming languages that require step-by-step instructions to perform tasks. Terraform providers automatically calculate dependencies between resources to create or destroy them in the correct order.

# Providers

-   Terraform plugins called _providers_ let Terraform interact with cloud platforms and other services via their APIs
-   There are over 1,000 providers available to manage resources (AWS, Azure, Google, Kubernetes, Github, Helm...)
-   Find providers for many of the platforms and services you already use in the [Terraform Registry](https://registry.terraform.io/browse/providers).
    -   If you don't find the provider you're looking for, you can write your own.

# State file

Terraform keeps track of your real infrastructure in a state file, which acts as a source of truth for your environment. Terraform uses the state file to determine the changes to make to your infrastructure so that it will match your configuration.
