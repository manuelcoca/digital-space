# Static Website Hosting

Azure Storage allows to serve static website content (HTML, CSS, JavaScript and image files) directly from a storage container named $web.

Static website hosting with Azure Storage is a great option in cases where you don't require a web server to render content.

Limitations:

-   Headers can't be configured
-   AuthN and AuthZ is not possible - If these features are important -> use Azure Static Web Apps
-   Doesn't support HTTPS with custom Domains natively - Use Azure CDN

## Enable static website hosting

1. Locate your storage account in the Azure portal and display the account overview
2. Select **Static website** to display the configuration page
3. Select **Enabled** to enable static website hosting for the account
4. In the **Index document name** field, specify a default index page. For example, _index.html_.
5. In the **Error document path** field, specify a default error page. For example, _404.html_.
6. Select **Save**.

![Screenshot showing the locations of the fields to enable and configure static website hosting.](https://learn.microsoft.com/en-us/training/wwl-azure/explore-azure-blob-storage/media/enable-static-website-hosting.png)

## Access level

Modifying the public access level of the `$web` container has no impact on the static website endpoint. The files inside `$web` container has public (read-only) access.

## Custom Domain

Custom Domains can only be used via HTTP because HTTPS with custom domain is not supported natively. Use Azure CDN to configure HTTPS with custom Domains -> [https://learn.microsoft.com/en-us/azure/storage/blobs/storage-custom-domain-name](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-custom-domain-name)
