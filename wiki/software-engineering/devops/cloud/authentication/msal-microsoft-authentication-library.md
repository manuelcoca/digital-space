---
tags: azure, azureauth, azuremsal
---

MSAL can be used to provide secure access to Microsoft Graph, other Microsoft APIs, third-party web APIs, or your own web API.

Benefits of using MSAL:

-   No need to directly use the OAuth libraries or code against the protocol in your application.
-   Acquires tokens on behalf of a user or on behalf of an application (when applicable to the platform).
-   Maintains a token cache and refreshes tokens for you when they're close to expire. You don't need to handle token expiration on your own.

# Authentication flows

The following table shows some of the different authentication flows provided by MSAL. These flows can be used in various application scenarios:

| Flow               | Description                                                                   |
| ------------------ | ----------------------------------------------------------------------------- |
| Authorization code | Native and web apps securely obtain tokens in the name of the user            |
| Client credentials | Service applications run without user interaction                             |
| On-behalf-of       | The application calls a service/web API, which in turns calls Microsoft Graph |
| Implicit           | Used in browser-based applications                                            |
| Device code        | Enables sign-in to a device by using another device that has a browser        |
| Integrated Windows | Windows computers silently acquire an access token when they're domain joined |
| Interactive        | Mobile and desktops applications call Microsoft Graph in the name of a user   |
| Username/password  | The application signs in a user by using their username and password          |
