# Azure Container Apps

Azure Container Apps is built on top of Kubernetes and abstracts the overhead of infrastructure management and orchestration. Azure Container Apps is a great choice for:

* Microservices
* APIs
* Event processing
* Long running background jobs

and more.&#x20;

## Features

Applications built on Azure Container Apps has a rich-feature set:

* Dynamic sacling based on: HTTP traffic, event-driven processing, CPU or memory load
* Enable HTTPS ingress without having to manage other Azure infrastructure.
* Split traffic across multiple versions of an application for Blue/Green deployments
* Use internal ingress and service discovery for secure internal-only endpoints with built-in DNS-based service discovery
* Build microservices with [Dapr](https://docs.dapr.io/concepts/overview/) and access its rich set of APIs.
* Run containers from any registry, public or private, including Docker Hub and Azure Container Registry (ACR)
* Use the Azure CLI extension, Azure portal or ARM templates to manage your applications
* Securely manage secrets directly in your application.
* Monitor logs using Azure Log Analytics.

and more.

## Azure Container Apps Environment

Individual container apps are deployed to a single Container Apps environment, which acts as a secure boundary around groups of container apps.

Reasons to deploy container apps to the same environment include situations when you need to:

* Manage related services
* Deploy different applications to the same virtual network
* Instrument [Dapr](https://docs.dapr.io/concepts/overview/) applications that communicate via the Dapr service invocation API
* Have applications to share the same Dapr configuration
* Have applications share the same log analytics workspace

Reasons to deploy container apps to different environments include situations when you want to ensure:

* Two applications never share the same compute resources
* Two Dapr applications can't communicate via the Dapr service invocation API

## AuthN & AuthZ with Azure Container Apps

Azure Container Apps provides built-in authentication and authorization features to secure your external ingress container app with minimal or no code. The built-in authentication feature for Container Apps can save you time and effort by providing out-of-the-box authentication with federated identity providers, allowing you to focus on the rest of your application.

* Azure Container Apps provides access to various built-in authentication providers (Microsoft, Github, Facebook etc.).
* The built-in auth features don’t require any particular language, SDK, security expertise, or even any code that you have to write.

This feature should only be used with HTTPS. Ensure `allowInsecure` is disabled on your container app's ingress configuration. You can configure your container app for authentication with or without restricting access to your site content and APIs.

* To restrict app access only to authenticated users, set its _Restrict access_ setting to **Require authentication**.
* To authenticate but not restrict access, set its _Restrict access_ setting to **Allow unauthenticated** access.

### Architecture

The platform middleware handles several things for your app:

* Authenticates users and clients with the specified identity provider(s)
* Manages the authenticated session
* Injects identity information into HTTP request headers

<div align="left">

<figure><img src="../../../.gitbook/assets/Screenshot 2023-07-07 at 16.43.28.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

\
\


