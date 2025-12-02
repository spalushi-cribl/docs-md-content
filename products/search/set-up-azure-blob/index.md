# Connect Cribl Search to Azure Blob Storage


Configure Cribl Search to query your Azure Blob Storage data.

---

[Azure Blob Storage](https://azure.microsoft.com/en-us/products/storage/blobs/) enables users to store large amounts of
unstructured data on Microsoft's data storage platform. Blobs are binary large objects, including images and multimedia.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a
[Dataset](basic-concepts#datasets) to [search](search-overview) objects in Azure Blob Storage.

Data transfer costs and other charges might apply. For details, see [Data Transfer Charges](#charges) below.

## Add an Azure Blob Storage Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an Azure Blob
Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.


![Azure Blob Storage Dataset Provider configuration](dataset-providers-selection-azure.png)
{scale="100%"}

Configure the **New Dataset Provider** modal as follows:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional. Here, you can enter a summary that will clarify this Dataset Provider's purpose to other
   users.
1. Set the **Dataset Provider Type** to `Azure Blob`.
1. **Authentication method** provides three options, **Connection String**, **Blob SAS URL**, and **Client Secret**. For
   details on each option, see the [next section](#auth).
1. Select **Save** when finished.

### Azure Blob Storage Authentication {#auth}

Your Azure Blob Storage access policy should include the **Read** and **List** [permissions](https://learn.microsoft.com/en-us/rest/api/storageservices/create-service-sas#permissions-for-a-directory-container-or-blob).

In the Cribl Search **New Dataset Provider** modal, use the **Authentication method** buttons to select one of these options:

- [Connection String](#connection-string)
- [Blob SAS URL](#blob-sas-url)
- [Client Secret](#client-secret)

#### Connection String {#connection-string}

The **Connection String** gives admin-level access to Search. This includes the authorization information required to
access data in your Azure Storage account at runtime using Shared Key authorization. See the
[Azure Blob Storage documentation](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage?tabs=azure-portal)
for more details. To view and copy your storage account connection string from the Azure portal:

1. In the Azure portal, go to your storage account.
1. Under **Security + networking**, select **Access keys**. Your account access keys appear, as well as the complete
   connection string for each key.
1. Select **Show keys** to show your access keys and connection strings and to enable buttons to copy the values.
1. Copy the entire **Connection string** of the key you want to use.

In the Cribl Search **New Dataset Provider** modal, enter the following information under **Authentication method**.

1. **Location** is your Azure account region, for example, `East US`. This can be found on the Azure portal **Overview**
   page. See the
   [Azure Blob Storage documentation](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-overview) for
   more details.
1. **Connection String** is where you paste the **Connection string** of the key you want to use.
1. Select **Save** when finished.

#### Blob SAS URL {#blob-sas-url}

**Blob SAS URL** provides a more-restrictive access token called a “Shared Access Signature”, which allows the user to
specify which resources to grant access to. For details see,
[Create SAS tokens for your storage containers](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/document-translation/how-to-guides/create-sas-tokens?tabs=Containers).
To create SAS tokens for your storage containers:

1. In the Azure portal, navigate to your container.
1. Right-click the container or file and select **Generate SAS** from the drop-down menu.
1. Specify the permission levels, allowed IP addresses, and allowed protocols.
1. Review and then select **Generate SAS token and URL**.
1. The **Blob SAS token** query string and **Blob SAS URL** are displayed in the lower area of the window.
1. Copy and paste the **Blob SAS token** and **URL** values in a secure location. They'll only be displayed once and
   cannot be retrieved once the window is closed.

In the Cribl Search **New Dataset Provider** modal, enter the following information under **Authentication method**.

1. **Location** is your Azure account region, for example, `East US`. This can be found on the Azure portal **Overview**
   page. See the
   [Azure Blob Storage documentation](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-overview) for
   more details.
1. Select **Add configurations** to specify the container(s).
   - **Container Name** is the name of the Azure Blob Storage container.
   - **Blob SAS URL** is where you paste the container-specific Blob SAS URL.
1. Select **Save** when finished.

#### Client Secret {#client-secret}

The **Client Secret** is a type of authentication available for a
[service principal](https://learn.microsoft.com/en-us/entra/architecture/service-accounts-principal) of your
[Microsoft Entra](https://www.microsoft.com/en-us/security/business/microsoft-entra) (formerly Azure Active Directory)
application. To use this authentication method, first do the following in your Azure account:

1. Create and register a service principal for your Microsoft Entra application. To find out how, see
   [Register a Microsoft Entra app and create a service principal](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal)
   from Microsoft.
1. Grant the service principal access to your data. To find out how, see
   [Authorize access to blobs using Microsoft Entra ID](https://learn.microsoft.com/en-us/azure/storage/blobs/authorize-access-azure-active-directory).

In the Cribl Search **New Dataset Provider** modal, enter the following information under **Authentication method**.

1. **Location** is your Azure account region, for example, `East US`.
1. **Storage account name** is the name of your Azure
   [storage account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview).
1. **Tenant ID** is your Microsoft Entra tenant ID. To find it, see
   [How to find your Microsoft Entra tenant ID](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-find-tenant).
1. **Client ID** is the client ID of the Azure service principal. To find it, see
   [Sign in to the application](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal#sign-in-to-the-application).
1. **Client secret** is the secret to use when connecting to your Microsoft Entra application. To get it, see
   [Create a new client secret](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal#option-3-create-a-new-client-secret).

## Add an Azure Blob Storage Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.


![Datasets selection](datasets-selection.png)
{scale="100%"}

Configure the **New Dataset** modal as follows.

### Identify the Dataset {#id}

Use the first few fields to uniquely identify this Dataset and specify its type:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`). You'll use this to specify the Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset (for example, `dataset=my_dataset_1`).
1. **Description** is optional. Here, you can enter a summary that will clarify this Dataset's purpose to other users.
1. Set **Dataset Provider** to the **ID** of an [Azure Blob Dataset Provider](#provider).

### Set Up Paths, Regions, Partitioning {#path}

Define at least one path, region, and partitioning configuration:

1. **Skip event time filter** is optional. Toggle this on only if you're sure you want to set the time range of your
   searches [by partition time-boundaries](#time-by-partition) rather than events' timestamps
1. **Container path** is the Azure Blob Storage container with your data. See the
   [Azure Blob Storage documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/blob-containers-portal) for
   details.
1. **Path filter** is a JavaScript filter expression that is evaluated against the container. Defaults to `true`, which
   matches all data, but you can [customize](#filter) this value.
1. The **Partitioning scheme** defaults to `Defined in Path`. For Splunk-specific alternatives, see
   [Azure Blob Storage Partitioning Scheme](#partitioning-scheme).

#### Multiple Path Configs {#multi-path}

Select **Add container/path** to define as many other path configurations as you want for this Dataset. Each path can
have its own filter and partitioning scheme.

As with parallel Datasets, each partitioning scheme applies per path. When searching a Dataset, Cribl Search will search
all its configured paths. However: **a failed search on any of a Dataset's paths will fail the whole search.**

### Set Up Datatypes and Storage Classes {#process-accel}

On the **Processing** left tab, you specify **Datatypes** to break data down into discrete events and define fields so
they're ready to search.

Use the drop-down to set the first rule to `AWS Datatypes`. Optionally, click **Add Datatype Ruleset** to select more
rulesets – each of which contains rules applied to the data searched in your Dataset. For details, see
[Datatypes](datatypes).

On the **Storage classes** left tab, select the access tiers that you want this Dataset to search against. You can
minimize retrieval costs by selecting only warmer tiers. You must select at least one tier. For details, see
[Storage Classes](storage-classes).

### Save the Configuration {#save}

Bypass the **Usage** left tabs for now, and select **Save** when your configuration is complete.

## Path/Partitioning Considerations {#pathology}

This section offers details and guidance on configuring search paths, filters, and partitioning schemes.

### Container Paths {#bucket}

Each **Container path** specifies a separate blob of data within the Dataset. It defines the scope of this data, to narrow down what data is in the Dataset. This field supports tokens and key-value pairs. For example:

- `mycontainer/data` – where `data` becomes a field for all events of that Dataset.
- `mycontainer/${data}/${*}` – equivalent, the wildcarded path is skipped.
- `mycontainer/<key=value>/<someVarOfInterest>` – see [Hive‑Style Paths](#hive) just below.

#### Hive-Style Paths {#hive}

To search paths of the form `my-bucket/<key=value>/<someVarOfInterest>`, your expression should place the wildcard `{*}`
in the ordinal position, to ignore the hive (K-V) segment:

`my-bucket/${*}/${someVarOfInterest}`

This will allow Cribl Search to find the wildcarded subdirectory automatically.




#### Basic Tokens

Basic tokens' syntax follows that of [JS template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals): `${token_name}` – where `token_name` is the field (name) of interest. 

For example, if the path was set to `/var/log/${hostname}/${dataSource}/`, you could use a filter such as `hostname=='myHost' && dataSource=='mydataSource'` to specify data only from the `/var/log/myHost/mydataSource/` subdirectory.




#### Time-Based Tokens

Paths with time notation can be referenced with tokens, having a direct effect on the earliest and latest boundaries. The supported time fields are:
  * `_time` is the raw event's timestamp.
  * `__earliest` is the search start time.
  * `__latest` is the search end time.

Time-based tokens are processed as follows:
  * For each path, times must be notated in descending order. So Year/Month/Day order is supported, but Day/Month/Year is not.
  * Paths may contain more than one time component. For example, `/my/path/2020-04/20/`.
  * In a given path, each time component can be used only once. So `/my/path/${_time:%Y}/${_time:%m}/${_time:%d}/...` is a valid expression format, but `/my/path/${_time:%Y}/${_time:%m}/${host}/${_time:%Y}/...` (with a repeated `Y`) is not supported.
  * For each path, all extracted dates/times are considered in UTC.
 
> Cribl recommends that your path always include and tokenize the largest available time fields, proceeding down to the smallest desired time fields. Otherwise, your searches might yield unexpected results, because Cribl Search will default the omitted fields to their earliest allowed value.
{.box .info}

The following `strptime` format components are allowed: 
  * `Y`, `y` for years
  * `m`, `B`, `b`, `e` for months
  * `d`, `j` for days 
  * `H`, `I` for hours
  * `M` for minutes
  * `S` for seconds
  * `s` for Unix-style Epoch times (seconds since 1/1/1970)

Time-based token syntax follows that of a slightly modified [JS template literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals):
`${_time: some_strptime_format_component}`. Examples:

Path | Matches
--- | ---
`/path/${_time:%Y}/${_time:%m}/${_time:%d}/...` | `/path/2020/04/20/...`
`/path/${_time:year=%Y}/${_time:month=%m}/${_time:day=%d}/...` | `/path/year=2020/month=05/day=20/...`
`/path/${_time:%Y-%m-%d}/...` | `/path/2020-05-20/...`


##### Set Query Time Range by Partition {#time-by-partition}

If your data is partitioned by time (so your paths probably use [time-based tokens](#time-based-tokens)), you can tell
Cribl Search to ignore events' timestamps, and instead set the time range using the partition time-boundaries.

For example, an event with the timestamp of `2025-07-01T23:59:00.000` might be stored in the July 2 partition. Queries
for July 1 or July 2 might not find it: the first due to wrong partition, the second due to wrong `_time`. Using
partition time for the query range avoids this issue.

To search by partition time range, toggle on **Skip event time filter** when setting up your [paths](#path). Then, any
time range you set (in the Cribl Search UI or with `earliest`/`latest` in the query text) will use partition time
instead of event time.


> If you toggle on the **Skip event time filter** option, Cribl Search will ignore the `_time` field in your events.
> Do **not** enable this option if your paths don't use time-based tokens.
{.box .warning}


### Azure Blob Storage Path Filter {#filter}

Each **Path filter** is a JavaScript expression that Cribl Search evaluates against the corresponding
**Container path**. The **Path filter** field's value defaults to `true`, which matches all data. However, you can
customize this value almost arbitrarily.

For example, if a Dataset has this **Path filter**:  
`source.endsWith('.log') || source.endsWith('.txt')`  
...this configuration will search only files/objects with `.log` or `.txt` extensions.

At the **Path filter** field's right edge are a Copy button and an Expand button that opens a validation modal.

With DDSS or SmartStore partitioning, you can provide the index name as **Path filter**, for example:
`index=="index_y"`.

### Azure Blob Storage Partitioning Scheme {#partitioning-scheme}

Each **Partitioning scheme** drop-down offers alternative options for partitioning inbound data from Splunk. You can
choose between [`Splunk DDSS`](https://docs.splunk.com/Documentation/SplunkCloud/9.1.2308/Admin/DataSelfStorage) and
[`Splunk SmartStore`](https://docs.splunk.com/Documentation/Splunk/9.1.2/Indexer/AboutSmartStore).

With either option, in the corresponding **Container path** field, provide the path where indexes are stored (that is,
your index's parent folder).


For Splunk DDSS, the full path takes the form:   
`<parent_folder>/<indexName>/db/db_<latestTime>_<earliestTime>_<bucketId>/rawdata/journal.gz|lz4|zst`
...or:  
`<parent_folder>/<indexName>/db_<latestTime>_<earliestTime>_<bucketId>/rawdata/journal.gz|lz4|zst`

For SmartStore, the full path takes the form:  
`<parent_folder>/<indexName>/db/<2-letter-hash>/<2-letter-hash>/<bucket_id_number-origin_guid>/<"guidSplunk"-uploader_guid>/`

With either option, if you organize your container using this default file path, Cribl Search will automatically
discover its content.

## Search Azure Blob Storage

Now that you have a Dataset Provider and Dataset, you're ready to start searching.

It can take a few minutes for a search to start returning results.

### Data Transfer Charges {#charges}

Cribl Search uses both Read and Iterative Read Operations in Azure Blob Storage. To understand the fees associated with
these operations, refer to Azure Blob Storage's
[Operation and data transfer](https://azure.microsoft.com/en-us/pricing/details/storage/blobs/).

Additionally, retrieval fees may apply depending on the storage tier used. For details, see Microsoft's
[Retrieval Fees](https://azure.microsoft.com/en-us/pricing/details/storage/blobs/) topic.

Using Cribl Search to read data from the Azure data centers might also result in Data transfer charges for egress. For
detailed information, refer to Microsoft's
[Data Transfer](https://azure.microsoft.com/en-us/pricing/details/bandwidth/#pricing) pricing.
