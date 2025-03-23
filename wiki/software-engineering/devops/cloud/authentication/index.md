---
tags: azure, azureauth
---

Azure Active Directoy is the main mechanism when it comes to identity and authentication within the Azure Cloud.

-   Azure AD is different than Windows AD
-   Azure AD Connect can be used to synchronize a on-premises Windows AD server to Azure AD

# Concept of Tenants

-   All Users belongs to at least one Tenant
    -   But User can belong to multiple Tenants (if you switch directories in Azure you bascially switch also the Tenant)
-   A tenant is the unit of Azure Active Directory and it's a security context
-   Each Tenant also has different Subscriptions
-   **`Global Administrator`** Role has top level permissions on a Tenant and can do everything

# App registration

Applications can be registered inside Azure AD via **`App Registration`** Tab in order to use SSO for authentication. If an application is registered in the portal, an application object and a service principal object are automatically created in the users home tenant.

## Application objects

Application objects are globally unique instances of the apps and are used as a template to create one or more service principal objects. Each app also have a globally unique ID (the app or client ID). It also describes three aspects of an application:

1. How the service can issue tokens in order to access the application
2. Resources that the application might need to access
3. The actions that the application can take

In the portal, the user can add secrets or certificates and scopes to make the app work, customize the branding of the app in the sign-in dialog, and more.

The Microsoft Graph [Application entity](https://learn.microsoft.com/en-us/graph/api/resources/application) defines the schema for an application object's properties.

# Service Principals

To access resources secured by an Azure Active Directory tenant, the entity that requires access must be represented by a security principal. This is true for both users (user principal) and applications (service principal).

The security principal defines the access policy and permissions for the user/application in the Azure Active Directory tenant. This enables core features such as authentication of the user/application during sign-in, and authorization during resource access.

There are 2 types of service principal:

-   **Application** - This type of service principal is the application instance, of a global application object in a single tenant or directory. A service principal is created in each tenant where the application is used and references the globally unique app object. The service principal object defines what the app can actually do in the specific tenant, who can access the app, and what resources the app can access.
-   **Managed identity** - This type of service principal is used to represent a managed identity (see [[managed-identities]]). When a managed identity is enabled, a service principal representing that managed identity is created in your tenant.

# Create a Tenant/Azure AD

## Basics:

Choose between 2 types of Azure AD:

1. **`Azure AD`**
2. **`Azure AD B2C (Business to Consumer)`**
    1. Social Media Accounts like LinkedIn can be used to authenticate

## Configuration

1. `Organization name`
    1. Use the company name
2. `Initial domain name`
    1. Set up initial domain name which can be later replaced by the company domain
3. `Country/Region`
    1. Is important for Data Protection requirements. If a country in EU data will not leave EU

# Configure a Tenant

## App registrations

Register application in order to use Azure AD for authentication in the application

-   Register application in order to use Azure AD for authentication in the application

1. Set up the Name of application
2. Select account types
    1. Single tenant
    2. Multitenant
    3. Multitenant and Microsoft accounts
        1. This type is necessary to use other Microsoft services like Skype, Xbox etc. to authenticate
3. Redirect URI
    1. Redirect URI within the application to return to with the token after successful authentication
4. Front-channel logout URL
    1. URL which will be called on logout (will not be rendered) to clear all the cookies and local data (also across tabs which means user will be logged out across tabs)

## API permission

Configure what API permissions the application should have. Which permissions the application has will be passed with the JWT.

Example of Microsoft Graph permissions to read all users in Azure AD:

1. Add permissions
2. Choose Microsoft Graph
3. Select permissions
    1. Directory.Read.All
    2. Group.Read.All
    3. User.Read
    4. User.Read.All
