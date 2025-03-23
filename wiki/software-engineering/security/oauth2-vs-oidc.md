---
tags:
    - softwareengineering
    - security
    - oauth2
    - oidc
---

# Comparison

OAuth2 (Open Authorization 2.0) and OIDC (OpenID Connect) are related but have distinct roles in authentication and authorization:

-   OAuth2 is mainly an authorization framework, granting third-party apps access to user resources without sharing credentials. It's used for scenarios like social media app access or third-party services accessing files.
-   OIDC, built on OAuth2, focuses on authentication. It securely authenticates users and provides identity info to applications. It's used for single sign-on (SSO) and obtaining user details post-authentication.

Key Differences:

1. **Purpose**:
    - OAuth2: Authorization
    - OIDC: Authentication & Identity
2. **Endpoints**:
    - OAuth2: Defines /authorize and /token for authorization but lacks standard user authentication endpoints.
    - OIDC: Adds /userinfo and /auth for user info retrieval and authentication.
3. **User Information**:
    - OAuth2: Doesn't define user info retrieval.
    - OIDC: Specifies user identity info with an ID Token (JWT).
4. **Use Cases**:
    - OAuth2: For granting access to resources without sharing credentials (e.g., API access).
    - OIDC: For user authentication and identity info retrieval (e.g., SSO, secure web app login).

In summary, OAuth2 handles authorization, while OIDC extends it for user authentication and identity retrieval. They are often used together when both authorization and authentication are required.

![[LbKkm.png]]

# Microsoft Azure AD B2C

Microsoft Azure AD B2C uses both the OAuth 2.0 and OpenID Connect protocols for authentication and authorization.

Here's how these protocols are used in Azure AD B2C:

1. **OAuth 2.0**: Azure AD B2C uses OAuth 2.0 for authorization. It allows applications to request and obtain access tokens that can be used to access protected resources or APIs on behalf of a user. Azure AD B2C supports the OAuth 2.0 Authorization Code Flow, Implicit Flow, Client Credentials Flow, and Resource Owner Password Credentials (ROPC) Flow, among others. These flows are used to handle various authentication and authorization scenarios for applications.
2. **OpenID Connect (OIDC)**: OIDC is an identity layer built on top of OAuth 2.0, and it is used for authentication. Azure AD B2C leverages OIDC to provide user authentication services. When a user signs in or registers with an application using Azure AD B2C, it issues an ID Token that contains information about the authenticated user. OIDC helps verify the user's identity and provides claims about the user, such as their email, name, and other profile information.

In addition to OAuth 2.0 and OIDC, Azure AD B2C also supports various other identity protocols and standards, including SAML (Security Assertion Markup Language), to provide flexibility for integrating with different types of applications and services.

In summary, Azure AD B2C primarily uses OAuth 2.0 and OIDC for authentication and authorization, making it suitable for a wide range of identity and access management scenarios, especially in the context of customer-facing applications and identity federation.
