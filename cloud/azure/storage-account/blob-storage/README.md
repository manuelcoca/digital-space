# Blob storage

Blob storage is optimized for storing massive amounts of unstructured data. Blob storage is designed for:

* Serving images or documents directly to a browser
* Storing files for distributed access
* Streaming video and audio
* Writing to log files
* Storing data for backup and restore, disaster recovery, and archiving
* Storing data for analysis by an on-premises or Azure-hosted service

Objects in Blob storage are accessible via the Azure Storage REST API, Azure PowerShell, Azure CLI, or an Azure Storage client library.

## Blob storage resource types

A storage account can include an unlimited number of containers, and a container can store an unlimited number of blobs.&#x20;

* Storage account
  * Container
    * Blob

#### Namespace

A storage account provides a unique namespace in Azure for your data. Every object that you store in Azure Storage has an address that includes your unique account name. Example:

{% hint style="info" %}
[http://mystorageaccount.blob.core.windows.net](http://mystorageaccount.blob.core.windows.net)
{% endhint %}

#### Containers

A container organizes a set of blobs, similar to a directory in a file system. The container name must be unique as well as it forms the URI. Example:

{% hint style="info" %}
[https://myaccount.blob.core.windows.net/mycontainer](https://myaccount.blob.core.windows.net/mycontainer)
{% endhint %}

#### Blob

Azure Storage supports three types of blobs:

* **Block blobs** store text and binary data. Block blobs are made up of blocks of data that can be managed individually. Block blobs can store up to about 190.7 TiB.
* **Append blobs** are made up of blocks like block blobs, but are optimized for append operations. Append blobs are ideal for scenarios such as logging data from virtual machines.
* **Page blobs** store random access files up to 8 TB in size. Page blobs store virtual hard drive (VHD) files and serve as disks for Azure virtual machines.

{% hint style="info" %}
[https://myaccount.blob.core.windows.net/mycontainer/myblob](https://myaccount.blob.core.windows.net/mycontainer/myblob)
{% endhint %}
