# Connect Cribl Search to Google Cloud Storage


Configure Cribl Search to query your Google Cloud Storage data.

---

[Google Cloud Storage](https://cloud.google.com/storage) is a managed service for storing unstructured data.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a
[Dataset](basic-concepts#datasets) to [search](search-overview) your Google Cloud Storage account.

Data transfer costs and other charges may apply. For details, see the [Data Transfer Charges](#charges) section below.

## Add a Google Cloud Storage Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add a Google
Cloud Storage Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.

Configure the **New Dataset Provider** modal as follows:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional.
1. Set **Dataset Provider Type** to **Google Cloud Storage**.
1. The **Service Account Credentials** are the contents of your Google Cloud
   [service account](https://cloud.google.com/iam/docs/service-account-overview) (JSON keys). You can also drag and drop
   a file into the field, or upload a file by clicking the upload button.<br>For more information on the permissions
   needed, see the [Google Cloud Permissions](#permissions) section.<br>For more information on obtaining your Google
   Cloud Storage credentials, see Google's
   [Authenticate to Cloud Storage](https://cloud.google.com/storage/docs/authentication).
1. Specify an **Endpoint**. If empty, the endpoint will default to `https://storage.googleapis.com`. For a list of the
   endpoints, see the Google Cloud Storage [API Reference](https://cloud.google.com/storage/docs/json_api/v1) doc.
1. Select **Save** when finished.

### Google Cloud Permissions {#permissions}

To let Cribl Search access your Google Cloud data, your Google Cloud
[service account](https://cloud.google.com/iam/docs/service-account-overview) needs at the minimum all of the following
roles:

- `Storage Legacy Bucket Reader`
- `Storage Legacy Object Reader`

Alternatively, you can define a custom role that has all of the following permissions:

- `storage.buckets.get`
- `storage.objects.list`

For more information, see Google's
[IAM roles for Cloud Storage](https://cloud.google.com/storage/docs/access-control/iam-roles).

## Add a Google Cloud Storage Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.

Configure the **New Dataset** modal as follows.

### Identify the Dataset {#id}

Use the first few fields to uniquely identify this Dataset and specify its type:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`). You'll use this to specify the Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset (for example, `dataset=my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of a [Google Cloud Storage Dataset Provider](#provider).

### Set Up Paths and Partitioning {#path}

Define at least one path, filter, and partitioning configuration:

1. **Skip event time filter** is optional. Toggle this on only if you're sure you want to set the time range of your
   searches [by partition time-boundaries](#time-by-partition) rather than events' timestamps
1. **Bucket Path** is the path to the Google Cloud Storage objects you'd like to search. Start the path with the bucket
   name, for example: `my-bucket/data/*`. Tokens and key-value pairs are supported. For details, see
   [bucket path](#bucket).
1. **Path Filter** is a JavaScript filter expression that is evaluated against the S3 bucket path. Defaults to `true`,
   which matches all data, but you can [customize](#filter) this value.
1. The **Partitioning scheme** defaults to `Defined in Path`. For Splunk-specific alternatives, see
   [Partitioning Scheme](#partitioning-scheme).

#### Multiple Path Configs {#multi-path}

Select **Add bucket/path** to define as many other path configurations as you want for this Dataset. Each path can have
its own filter and partitioning scheme.

As with parallel Datasets, each partitioning scheme applies per path. When searching a Dataset, Cribl Search will search
all its configured paths. However: **a failed search on any of a Dataset's paths will fail the whole search.**

### Set Up Datatypes and Storage Classes {#process-accel}

On the **Processing** left tab, you specify **Datatypes** to break data down into discrete events and define fields so
they're ready to search.

Use the drop-down to set the first rule to `AWS Datatypes`. Optionally, click **Add Datatype Ruleset** to select more
rulesets – each of which contains rules applied to the data searched in your Dataset. For details, see
[Datatypes](datatypes).

On the **Storage classes** left tab, select the storage classes that you want this Dataset to search against. You can
minimize retrieval costs by selecting only warmer classes. You must select at least one class. For details, see
[Storage Classes](storage-classes).

### Save the Configuration {#save}

Bypass the **Usage** left tabs for now, and select **Save** when your configuration is complete.

## Path/Partitioning Considerations

This section offers details and guidance on configuring search paths, filters, and partitioning schemes.

### Bucket Paths {#bucket}

The **Bucket Path** specifies the data that the Dataset consists of. It defines the scope of data, to narrow down what
data is in the Dataset. This is a JavaScript expression that supports tokens and key-value pairs. For example,

- `my-bucket/${data}/` – where `data` becomes a field for all events of that Dataset.
- `my-bucket/${data}/${*}` – equivalent, the wildcarded path is skipped.
- `my-bucket/<key=value>/<someVarOfInterest>` – see [Hive‑Style Paths](#hive) just below.

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


### Path Filter {#filter}

Each **Path filter** is a JavaScript expression that Cribl Search evaluates against the corresponding **Bucket path**.
The **Path filter** field's value defaults to `true`, which matches all data. However, you can customize this value
almost arbitrarily.

For example, with this **Path filter**:  
`source.endsWith('.log') || source.endsWith('.txt')`  
...this configuration will search only files/objects with `.log` or `.txt` extensions.

At each **Path filter** field's right edge are a Copy button and an Expand button that opens a validation modal.

With DDSS or SmartStore partitioning, you can provide the index name as the **Path filter**, for example:  
`index=="index_y"`

### Partitioning Scheme

Each **Partitioning scheme** drop-down offers alternative options for partitioning inbound data from Splunk. You can
choose between [`Splunk DDSS`](https://docs.splunk.com/Documentation/SplunkCloud/9.1.2308/Admin/DataSelfStorage) and
[`Splunk SmartStore`](https://docs.splunk.com/Documentation/Splunk/9.1.2/Indexer/AboutSmartStore).

With either option, in the corresponding **Bucket path** field, provide the path where indexes are stored (that is, your
index's parent folder).


For Splunk DDSS, the full path takes the form:   
`<parent_folder>/<indexName>/db/db_<latestTime>_<earliestTime>_<bucketId>/rawdata/journal.gz|lz4|zst`
...or:  
`<parent_folder>/<indexName>/db_<latestTime>_<earliestTime>_<bucketId>/rawdata/journal.gz|lz4|zst`

For SmartStore, the full path takes the form:  
`<parent_folder>/<indexName>/db/<2-letter-hash>/<2-letter-hash>/<bucket_id_number-origin_guid>/<"guidSplunk"-uploader_guid>/`

With either option, if you organize your bucket using this default file path, Cribl Search will automatically discover
its content.

## Search Google Cloud Storage

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-overview).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

### Data Transfer Charges {#charges}

Cribl Search uses both Class A and Class B operations in Google Cloud Storage. To understand the fees associated with
these operations, refer to the Google Cloud Storage's
[Operation charges](https://cloud.google.com/storage/pricing#operations-pricing).

Additionally, retrieval fees may apply depending on the storage class of the bucket. See the
[Retrieval Fees](https://cloud.google.com/storage/pricing#retrieval-pricing) section for details.

Using Cribl Search to read data from US regions might also result in General network usage egress charges. These charges
fall under the "Egress Worldwide Destinations (excluding Asia & Australia)" category. For detailed information, refer to
the [General network usage](https://cloud.google.com/storage/pricing#network-egress).
