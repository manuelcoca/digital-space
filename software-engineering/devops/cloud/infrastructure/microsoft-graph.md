# Microsoft Graph

Microsoft Graph exposes REST APIs and client libraries to access data on the following Microsoft cloud services:

* Microsoft 365 core services: Calendar, Excel, OneDrive, OneNote, Outlook, Sharepoint, Teams etc.
* Azure Active Directory
* Intune
* And more

## Accessing Azure AD

Grant API permissions to your application to access Azure AD. Navigate to Azure AD > App registrations. Register App and add API permissions for Microsoft Graph (read also [here](../../../../cloud/azure/broken-reference/)):

1. \+ Add permissions
2. Choose Microsoft Graph
3. Select permissions
   1. Directory.Read.All
   2. Group.Read.All
   3. User.Read
   4. User.Read.All
4. Create new Secret for application under the Section **`Certificates & Secrets`** within an App registration
5. Use Secret inside your Code to access Microsoft Graph API. See Code samples -> [https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2)
