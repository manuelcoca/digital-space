# AzCopy

AzCopy is a command line utility to copy files between Blob Storages (Containers), Storage Accounts, Subscriptions or even between different Clouds. If copying within Azure, all this happens on the server side in the cloud, so moving large files is not a problem as it would be to move over the internet.

## Installation

AzCopy can either be installed locally or used within the Azure Portal via the Command Line. - -&#x20;

Downloadlink: [https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10#download-azcopy](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10#download-azcopy)

## Usage

AzCopy users can be authorized by Azure AD or SAS token.&#x20;

#### Example copying files between Blob Storage:

* To authenticate via SAS, simply generate an SAS URL for the source file and another SAS URL for the destination Container with write access
* Run command:
  * **`azcopy copy <source-url> <destination-url>`**

See all commands and examples:

* [https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10#transfer-data](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10#transfer-data)

## Copy files programatically

Install NuGet Package Azure.Storage.Blob and use like in the example:

[https://www.nuget.org/packages/Azure.Storage.Blobs/](https://www.nuget.org/packages/Azure.Storage.Blobs/)
