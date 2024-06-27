# Built-in AuthN & AuthZ

Azure App Service provides built-in authentication and authorization support, so you can sign in users and access data by writing minimal, or no code in your web app, RESTful API, mobile back end, and Azure Functions. Advantages:

* It’s built directly into the platform and doesn’t require any particular language, SDK, security expertise, or code.
* You can integrate with multiple login providers. For example, Azure AD, Facebook, Google, Twitter.

## Identity Providers

<table><thead><tr><th width="199.33333333333331">Provider</th><th width="303">Sign-in endpoint</th><th>How-To guidance</th></tr></thead><tbody><tr><td>Microsoft Identity Platform</td><td><code>/.auth/login/aad</code></td><td><a href="https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad">App Service Microsoft Identity Platform login</a></td></tr><tr><td>Facebook</td><td><code>/.auth/login/facebook</code></td><td><a href="https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-facebook">App Service Facebook login</a></td></tr><tr><td>Google</td><td><code>/.auth/login/google</code></td><td><a href="https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-google">App Service Google login</a></td></tr><tr><td>Twitter</td><td><code>/.auth/login/twitter</code></td><td><a href="https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-twitter">App Service Twitter login</a></td></tr><tr><td>Any OpenID Connect provider</td><td><code>/.auth/login/&#x3C;providerName></code></td><td><a href="https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-openid-connect">App Service OpenID Connect login</a></td></tr><tr><td>GitHub</td><td><code>/.auth/login/github</code></td><td><a href="https://learn.microsoft.com/en-us/azure/app-service/configure-authentication-provider-github">App Service GitHub login</a></td></tr></tbody></table>

## How it works

The authentication and authorization module runs in the same sandbox as your application code. When it's enabled, every incoming HTTP request passes through it before being handled by your application code. This module handles several things for your app:

* Authenticates users and clients with the specified identity provider(s)
* Validates, stores, and refreshes OAuth tokens issued by the configured identity provider(s)
* Manages the authenticated session
* Injects identity information into HTTP request headers

## Authentication Flow

The authentication flow is the same for all providers, but differs depending on whether you want to sign in with the provider's SDK.

* Without provider SDK: The application delegates federated sign-in to App Service. This is typically the case with browser apps, which can present the provider's login page to the user. The server code manages the sign-in process, so it's also called _server-directed flow_ or _server flow_.
* With provider SDK: The application signs users in to the provider manually and then submits the authentication token to App Service for validation. This is typically the case with browser-less apps, which can't present the provider's sign-in page to the user. The application code manages the sign-in process, so it's also called _client-directed flow_ or _client flow_. This applies to REST APIs, Azure Functions, JavaScript browser clients, and native mobile apps that sign users in using the provider's SDK.

## Authorization behaviour

* **Allow unauthenticated requests:** This option defers authorization of unauthenticated traffic to your application code. For authenticated requests, App Service also passes along authentication information in the HTTP headers. This option provides more flexibility in handling anonymous requests. It lets you present multiple sign-in providers to your users.
* **Require authentication:** This option rejects any unauthenticated traffic to your application. This rejection can be a redirect action to one of the configured identity providers. In these cases, a browser client is redirected to `/.auth/login/<provider>` for the provider you choose.

## Logging

If you enable application logging, authentication and authorization traces are collected directly in your log files. If you see an authentication error that you didn't expect, you can conveniently find all the details by looking in your existing application logs.
