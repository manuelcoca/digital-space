ACR is a Docker Image repository for storing and managing Docker Images. Developers can automate Container builds with triggers such as source code commits and base image updates by using ACR Tasks.

## Use-Cases

1. Pull image from ACR to different targets. Examples:
    - Kubernetes Cluster
    - Azure Serivce like AKS, App Service and more
2. Push and store images or target the ACR from a Continious Delivery (CD) Tool like Azure Pipeline or Jenkins
3. Configure ACR tasks to automate building and testing images

## Service tiers

All tiers has encryption-at-rest - Azure automatically encrypts an image before storing it, and decrypts it on-the-fly when you or your applications and services pull the image

1. Basic
    - A cost-optimized entry point for developers learning about Azure Container Registry. Basic registries have the same programmatic capabilities as Standard and Premium (such as Azure Active Directory authentication integration, image deletion, and webhooks). However, the included storage and image throughput are most appropriate for lower usage scenarios.
2. Standard
    - Standard registries offer the same capabilities as Basic, with increased included storage and image throughput. Standard registries should satisfy the needs of most production scenarios.
3. Premium
    - Premium registries provide the highest amount of included storage and concurrent operations, enabling high-volume scenarios. In addition to higher image throughput, Premium adds features such as geo-replication for managing a single registry across multiple regions, content trust for image tag signing, and private link with private endpoints to restrict access to the registry.

## Manage Images with ACR Tasks

ACR Tasks enables automated builds. Supported triggers:

1. Source code updates
2. Updates to a container's base image
3. Timers

## Pushing Docker Images from Visual Studio

1. Add Docker Support to the Project
2. Run the "Docker" Option which will build the image and test if Dockerfile is correct
3. Publish App (Build > Publish > New) and create a new Publish Profile by choosing ACR as a publishing target (Azure > ACR)

## Settings

1. Access keys
    - By default no one has access to the ACR
    - Activate admin user and use credentials to to deploy the Docker Images into Azure using command line, PowerShell or within the portal.
