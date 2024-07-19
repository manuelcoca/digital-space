# Azure Storage Account

Durable, high available and scaleable cloud storage. Inside an Storage Account different storage types can be created:

1. Blob Storage
2. Tables
3. Queues (Read [here](../../../../backend/messaging/storage-queues.md))
   1. An option for storing small bits of data in a queue (first in first out) for other applications to read. Example: Store application requests of an application in a queue.
4. Files shares

## Types of Storage Accounts

<table><thead><tr><th>Storage account type</th><th width="203.33333333333331">Performance tier</th><th>Usage</th></tr></thead><tbody><tr><td>General-purpose v2</td><td>Standard</td><td>Standard storage account type for blobs, file shares, queues, and tables. Recommended for most scenarios using Blob Storage or one of the other Azure Storage services.</td></tr><tr><td>Block blob</td><td>Premium</td><td>Premium storage account type for block blobs and append blobs. Recommended for scenarios with high transaction rates or that use smaller objects or require consistently low storage latency</td></tr><tr><td>Page blobs</td><td>Premium</td><td>Premium storage account type for page blobs only.</td></tr></tbody></table>

## Access tiers

Data sets have unique lifecycles. Some data expires days or months after creation, while other data sets are actively read and modified throughout their lifetimes.

Azure storage offers different access tiers, allowing you to store blob object data in the most cost-effective manner. Available access tiers include:

* **Hot**
  * Optimized for storing data that is accessed frequently
  * Highest storage costs but lowest access costs
  * New Storage accounts are created in hot tier by default
* **Cool**
  * Optimized for storing data that is infrequently accessed and stored for at least 30 days
  * Lower storage costs but higher access costs compared to Hot tier.
* **Archive**
  * Available only for individual block blobs
  * Optimized for data that can tolerate several hours of retrieval latency and will remain in the Archive tier for at least 180 days
  * Most cost-effective option for storing data, but high costs for accessing data

Read more under [Lifecycle Management](lifecycle-management.md).

## Storage Account security features

* All data written and persistet to Azure Storage is encrypted (including metadata) using Storage Service Encryption (SSE)
* Data can be secured in transit between an application and Azure by using HTTPS
* OS and data disks used by Azure virtual machines can be encrypted using Azure Disk Encryption
* Delegated access to the data objects in Azure Storage can be granted using a [shared access signature (SAS)](./#shared-access-signature-sas)
* Storage accounts are encrypted regardless of their performance tier (Standard or Premium), resource types etc. and encryption doesn't affect performance or costs

## Create Storage Accounts

Storage accounts are provisioned by Storage (per GB) and Access (Write and Read operations).

{% tabs %}
{% tab title="Basics" %}
1. **`Region`**
   * Each Region has differents costs and different Redundancy Zones
2. **`Performance`**
   1. Standard (see above)
   2. Premium (see above)
3. **`Redundancy`**
   * LRS - Locally-redundant storage = Keeps 3 copies locally on the same data center
   * GRS - Geo-redundant storage = Keeps 6 copies, 3 locally and 3 in another region
   * ZRS - Zone-redundant storage = Keeps 3 copies in 3 different regions
   * GZRS - Geo-zone-redundant storage = 3 copies are stored zone redundantly and 3 geo redundantly
   * Checkbox - If checked redundant files can be accessed by a second URL
{% endtab %}

{% tab title="Advanced" %}
1. **`Require secure transfer for REST API operation`**
   * Requires HTTPS to interact via REST API
2. **`Enable Blob public access`**
   * If checked blob storages can be public accessible
3. **`Enable storage account key access`**
   1. Default access method - Azure will create 2 public keys which can be used to access the storage
4. **`Azure Active Directory`**
   1. Setup Azure AD to access the storage
5. **`Access Tier`**
   1. Read above
{% endtab %}

{% tab title="Networking" %}
1. **`Network access`**
   * Public access - Storage can be accessed from the internet
   * Public access from selected networks - Storage can only be accessed within Azure networks
   * Disable public access - Storage only accessable via private endpoints
2. **`Network routing`**
   * Microsoft Network routing - Data will be routed through Microsofts internal network. Public internet is minimized.
   * Internet routing - Data is routed through public internet
{% endtab %}

{% tab title="Data Protection" %}
1. **`Enable point in time restore`**
   * Allows to restore data back in time
2. **`Enable soft delete for blobs/container/file shared`**
   * File is not deleted immediately, instead it's deleted after a set amount of time - For protecting against accidental deletions
3. **`Tracking`**
   * Versioning - Enable file versioning for better security. Is not recommended for files that are accessed high frequently
   * Blob Change Feed - Send notficiations about changes and see what was changed
4. **`Access control`**
   * Make files immutable. Files cannot be changed. Helpful for regulatory challenges
{% endtab %}

{% tab title="Encryption" %}
1. **`Encryption type`** - Files are always encrypted, but with the Encryption type the user can control rather he or Microsoft controls the keys
   * Microsoft Managed Key (MMK) - Microsoft control the keys
   * Customer Managed Keys (CMK) - Setup own Key-Vault and connect to it
2. **`Enable infrastructure encryption`**
   * Enables encryption between the Azure Services and the hard disk where the files are stored
{% endtab %}
{% endtabs %}

## Menu/Configuartion

<details>

<summary>Containers</summary>

For creating a Blob Storage. For each container and Access Level can be configured, either private or public. It can only be public, if also the Storage Account is public.

</details>

<details>

<summary>Storage Browser</summary>

Tool for viewing data, even if access leve is set to private

</details>

<details>

<summary>Access keys</summary>

Access keys authenticate your applications requests to this storage account. It's not recommended to give access keys to users and they should be stored secuerly e.g in Azure Key-Vault. With the rotate function the keys can be changed frequently to avoid security risks.

To give access to users, use Shared access signature

</details>

<details>

<summary>Shared access signature (SAS)</summary>

* An shared access signature allows very fine-grained access control. It generates a token an an URL for accessing the Storage Account based on the configuration made.
* An SAS URL can also be created for each file seperately.
* Revoke access to an SAS URL by regenerating the Signing Key. The SAS Token and URL is signed with an Access Key, which means a new Access Key needs to be generated to make the SAS URL invalid.

Read more about [SAS](../../../../cloud/azure/storage-account/broken-reference/).

</details>

<details>

<summary>Data protection</summary>

1. **`Enable operational Backup with Azure Backup`** to create backups
   * In order to create backups a **`Backup-Vault`** needs to be created
   * And also **`Backup Policy`** needs to be created to decide how often backup should run

</details>

<details>

<summary>Object replication</summary>

By default, Micrsoft stores file replicas (always min. of 3 files) as defined by the Redundancy plan (see above). With Object replication, custom replica rules can be configured to decide how many replicas and where to store them

</details>

<details>

<summary>Lifecycle management</summary>

Lifecycle management allows to set up a rule that will move files from the hot tier to the cool tier or to the archive tier based on the last access date. Read more [here](lifecycle-management.md).

</details>

###
