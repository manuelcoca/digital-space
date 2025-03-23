---
tags: terraform
---

A configuration can consist of multiple files and directories.

# Modules

-   Is a collection of `.tf` and/or `.tf.json` files in a directory
-   Only consists of the top-level configuration files in a directory; nested directories are treated as completely separate modules
-   All file in a module are evaluated, but whole module is treated as a single document
-   Separating blocks into different files is a good pattern
-   A module can import other modules from
    -   local directory
    -   local disk
    -   or Terraform Registry via Link

# Root Module

-   Terraform always runs in the context of a single root module
    -   A complete Terraform configuration consists of a root module and the tree of child modules (which includes the modules called by the root module, any modules called by those modules, etc.).
-   Root module is where the working directory where initialised via `terraform init`

# Configuration File Structure

A configuration file is basically built around two key concepts:

-   Arguments
-   Blocks

## Arguments

Arguments assign a value to a particular name:
`image_id = "abc123"`

Argument values can include function calls, operators and much more. Read [here](https://developer.hashicorp.com/terraform/language/expressions).

## Blocks

A block is a container for other content:

```
resource "aws_instance" "example" {
  ami = "abc123"

  network_interface {
    # ...
  }
}
```

A block has a *type* (`resource` in this example). Each block type defines how many *labels* must follow the type keyword. The `resource` block type expects two labels, which are `aws_instance` and `example` in the example above.

Within the block body, other blocks (nested blocks) can be defined.

The Terraform language uses a limited number of *top-level block types,* which are blocks that can appear outside of any other block in a configuration file. Most of Terraform's features (including resources, input variables, output values, data sources, etc.) are implemented as top-level blocks.

# Examples

### Azure Storage Account

```
resource "azurerm_storage_account" "dev-storage" {
  name                     = "segdevstoragepd"
  location                 = var.location
  resource_group_name      = var.resource_group_name
  account_tier             = "Standard"
  account_replication_type = "LRS"

  tags = var.tags
}
```

### Define Docker Image and create Container

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```
