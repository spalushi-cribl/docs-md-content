# Connect Cribl Search to Amazon Security Lake


Configure Cribl Search to query your Amazon Security Lake data.

---

Using [Amazon Security Lake](https://aws.amazon.com/security-lake/), you can automatically consolidate security data
from across AWS environments, SaaS providers, on-prem systems, and cloud sources into a purpose-built data lake.

In this guide, you'll set up a [Dataset Provider](basic-concepts#dataset-providers) and a [Dataset](basic-concepts#datasets) to
[search](search-overview) objects in your Amazon Security Lake.

Data transfer costs and other charges may apply. For details, see the [Data Transfer Charges](#charges) section below.

## Add an Amazon Security Lake Dataset Provider {#provider}

A Dataset Provider tells Cribl Search where to query and contains access credentials. Here, you will add an Amazon
Security Lake Dataset Provider.

To add a new Dataset Provider, select **Data**, then **Dataset Providers**, then **Add Provider**.


![Amazon Security Lake Dataset Provider selection](dataset-providers-selection-security-lake.png)
{scale="100%"}

### Basic Configuration {#basic}

Configure the **New Dataset Provider** modal as follows:

1. **ID** is a unique identifier for the Dataset Provider. This is how you'll reference it when assigning Datasets to
   it. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_provider_1`).
1. **Description** is optional. Here, you can enter a summary that will clarify this Dataset Provider's purpose to other
   users.
1. Set the **Dataset Provider type** to `Amazon S3`.
1. **Authentication method** provides two options, **Assume Role** and **AWS keys**. For details on configuring each option, see [Authentication Method](#auth) below. 

### Advanced Settings {#advanced}

This section provides the following optional configurations:

- **Endpoint**: S3 service or compatible endpoint. If empty, defaults to AWS Region-specific endpoint.

- **Signature version**: Signature version to use for signing S3 requests. Defaults to `v4`.

- **Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

- **Reject unauthorized certificates**: Whether to reject certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on.

- **Apply ABAC source‑ip tagging**: Displayed only if you've set the **Authentication method** to **AssumeRole**. For details, see [ABAC Tagging](#abac) below.

### Wrapping Up {#wrap-up}

Select **Save** when your configuration is complete.

### Authentication Method {#auth}

Your choices here are **AssumeRole** or **AWS keys**. For rich details on both options, beyond the configuration basics
in this section, see [Grant Access to AWS](aws-access).

##### AssumeRole Authentication {#assume-role}

Selecting **AssumeRole** requires the IAM role's ARN (**AssumeRole ARN**), and also exposes the following options:

- The **External ID** on this Dataset Provider must match the
  [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) defined in
  the IAM Role [Trust Policy](aws-access#trust).

- The **Duration (seconds)** defines how long the AssumeRole's session lasts. Minimum is `900` (15 minutes), default
  is `3600` (1 hour), and maximum is `43200` (12 hours).

- The [ABAC tagging](#abac) controls described in the next section.

To the right, select the **Trust Policy** and **Permission Policy** buttons to
see the default policies that you can copy and expand, so you don't
have to write them from scratch. For more information, see [Example Trust Policy
](aws-access#trust-example) and [Expand the Permission Policy](aws-access#permission-vpce-condition).

##### AWS Keys Authentication {#aws-keys}

Selecting **AWS keys** requires the IAM user's **Access key** and
**Secret key**.

To the right, select the **Permission Policy** button to see the default policy
that you can copy and expand, so you don't have to write it from scratch.
For more information, see [Expand the Permission Policy](aws-access#permission-vpce-condition).

#### ABAC Tagging {#abac}

Select **Apply ABAC source‑ip tagging** if you want to make your AssumeRole sessions more secure by enabling attribute-based access controls (ABAC). By enabling this option, you can hard-code Cribl's Virtual Private Cloud Endpoint (VPCE) into your IAM trust policy.

Selecting this option exposes **VPC Endpoint IDs**. Use the adjacent Copy button to transfer the VPCE ID to your IAM trust policy, as shown in [Trust Policy with ABAC Tagging](aws-access#trust-vpce).

Completing both parts of this configuration will make your credentials harder to impersonate. Specifically, your AssumeRole sessions will include a `source-ip` tag, enabling you to enforce a condition that `aws:RequestTag/source-ip` equals `${aws:SourceIp}`.

## Add an Amazon Security Lake Dataset {#dataset}

Now you'll add a Dataset that tells Cribl Search what data to search from the Dataset Provider.

To add a new Dataset, select **Data**, then **Datasets**, then **Add Dataset**.


![Datasets selection](datasets-selection.png)
{scale="100%"}

Configure the **New Dataset** modal as follows.

### Identify the Dataset {#id}

Use the first few fields to uniquely identify this Dataset and specify its type:

1. **ID** is an identifier unique for both Cribl Search and [Cribl Lake](/lake/datasets). You'll use this to specify the
   Dataset in a query's [scope](build-a-search#syntax), telling Cribl Search to search the Dataset. Start the ID with a letter; the rest of the ID can use letters, numbers, and underscores (for example, `my_dataset_1`).
1. **Description** is optional.
1. Set **Dataset Provider** to the **ID** of an [Amazon Security Lake Dataset Provider](#provider).

### Set the Path and Filter {#path}

Define the Dataset's bucket path and filtering:

1. **Path** is the path within the S3 buckets you’d like to search. Tokens and key-value pairs are supported. The path
   is prefilled with a recommended path string, that you can use for guidance. For details, see
   [Set Path for Amazon Security Lake](#bucket).
1. **Path Filter** is a JavaScript filter expression that is evaluated against the S3 bucket path. It defaults to
   `true`, which matches all data, but you can [customize](#filter) this value.

### Set Up Buckets and Regions {#multi-bucket}

Under **Selected buckets**, define at least one S3 bucket/Region pair for this Dataset to search:

1. Select **Add bucket** to add the first pair. In the resulting table row:
1. **Bucket name** is the name of the S3 bucket.
1. **Region** is the AWS Region where the bucket is located.

Select **Add bucket** again for each additional bucket/Region pair you want to append to the table.

### Set Up Datatypes {#process-accel}

1. On the **Processing** left tab, you specify **Datatypes** to break data down into discrete events and define fields
   so they're ready to search. Use the drop-down to set the first rule to `OCSF Datatypes`. For details, see
   [Datatypes](datatypes).

1. Optionally, click **Add Datatype Ruleset** to define more rulesets – each of which contains rules applied to the data
   searched in your Dataset.

1. Create a rule on a Datatype that converts the time field from milliseconds to seconds (For example,
   `_time = time/1000`). For details, see the [Assign `time` Field](#example) section below.

### Save the Configuration {#save}

Bypass the **Usage** left tabs for now, and select **Save** when your configuration is complete.

## Path/Filter Considerations {#pathology}

This section offers details and guidance on configuring the search path, filter, and time extraction.

### Set Path for Amazon Security Lake {#bucket}

The **Path** specifies the data that the Dataset consists of. It defines the scope of data, to narrow down what data is
in the Dataset. This is a JavaScript expression that supports tokens and key-value pairs. For example,

- `path/to/folder/${data}/` – where `data` becomes a field for all events of that Dataset.
- `path/to/folder/${data}/${*}` – where `data` and the wildcarded path becomes a field for all events of that Dataset.




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

### Set Path Filter for Amazon Security Lake {#filter}

The **Path filter** is a JavaScript filter expression that Cribl Search evaluates against the corresponding
**Container path**. The **Path filter** field's value defaults to `true`, which matches all data. However, you can
customize this value almost arbitrarily.

For example, if a Dataset has this **Filter**:  
`source.endsWith('.log') || source.endsWith('.txt')`  
...then only files/objects with `.log` or `.txt` extensions will be searched.

At the **Path filter** field's right edge are a Copy button and an Expand button that opens a validation modal.

### Assign `time` Field for Amazon Security Lake {#example}

Amazon Security Lake automatically partitions incoming data from natively supported AWS services into storage-efficient
and query-efficient Parquet files. Additionally, it transforms data from native AWS services into the open-source OCSF
format.

The time is assigned to the `time` field, which is an epoch time expressed in milliseconds.

To use this field, you need to set its value to the Cribl Search `_time` field. Because Cribl Search uses seconds
instead of milliseconds, you must also convert it by dividing by 1,000. For this to work, add this field to a
Datatype:  
`_time` = `time/1000`.

## Search Amazon Security Lake

Now that you have a Dataset Provider and Dataset, you're ready to start [searching](search-overview).

Search results can start showing up within a second or two, but when the search completes depends on how much data there
is in the account.

### Data Transfer Charges {#charges}

Data transfer to Cribl Search is free if your S3 bucket is in the same Region as your
[Cribl.Cloud](getting-started-guide) [Workspace](workspaces).

Data transfer to Cribl Search is subject to AWS data transfer costs if your S3 bucket is not in the Regions supported by
Cribl.Cloud. See the **Data transfer** section in [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/).

Cribl Search will try to minimize the amount of data transferred, to the extend possible, by using path filters,
pruning, and other methods.

### Other Charges

Other charges may apply. For example, **LIST** and **GET** requests to your S3 buckets will be charged per AWS S3. See
the **Requests & data retrievals** section in [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/).

