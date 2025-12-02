# Amazon Security Lake Destination


This Destination delivers data to [Amazon Security Lake](https://docs.aws.amazon.com/security-lake/latest/userguide/what-is-security-lake.html), a fully-managed security data lake service backed by Amazon S3 buckets.

> Type: Non-Streaming | TLS Support: Yes | PQ Support: No
>
{.box .info}

Security Lake ingests events described by the [Open Cybersecurity Schema Framework](https://schema.ocsf.io/categories) (OCSF) and conveyed as Parquet files. This Destination sends the Parquet files in batches. Cribl Stream does not read or replay the data sent to Security Lake.

> Cribl recommends that you start with the [Amazon Security Lake Integration](usecase-security-lake) topic, which will guide you through the following steps in the correct order:
> 
> 1. Set up Amazon Security Lake.
> 1. Set up Cribl Stream.
> 1. Create an Amazon Security Lake [custom source](https://docs.aws.amazon.com/security-lake/latest/userguide/custom-sources.html), This is an S3 bucket dedicated to a single OCSF event class.
> 1. Create a Cribl Amazon Security Lake Destination.
> 1. Connect a Cribl Source to the Amazon Security Lake Destination.
> 1. Send data to Amazon Security Lake.
> 
> As shown above, you'll create and configure this Destination as the fourth of the six overall steps. The Integration topic covers the key points about configuring this Destination. Meanwhile, refer to the present topic as needed for details about any given setting.
>
{.box .success}

Although Cribl Stream does **not** need to be deployed in [Cribl.Cloud](deploy-cloud) in order to deliver data to Amazon Security Lake, setup procedures will differ from those documented here. To learn more, please contact Cribl via the `#packs` channel of Cribl Community Slack.

## Configuring Cribl Stream to Output to Amazon Security Lake

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Destination** at right. From the resulting drawer's tiles, select **Data Lakes** > **Amazon Security Lake**.  Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Data Lakes** > **Amazon Security Lake**. Next, click **Add Destination** to open a **New Destination** modal that provides the options below.

### General Settings

> The [Integration](usecase-security-lake) topic should be your primary guide to setting up Amazon Security Lake. For that reason, some of the instructions below assume that you have noted IDs, values, etc., from procedures you have already completed as directed by the Integration topic.
>
{.box .success}

**Output ID**: Enter a unique name to identify this Amazon Security Lake Destination definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**. 

**S3 bucket name**: Name of the destination S3 Bucket you created when [setting up Amazon Security Lake](usecase-security-lake#set-up-security-lake). This value can be a constant, or a JavaScript expression that will be evaluated only at init time. For example, referencing a Global Variable: `myBucket-${C.vars.myVar}`.

> Event-level variables are not available for JavaScript expressions. This is because the bucket name is evaluated only at Destination initialization.
>
{.box .info}

**Region**: Region where the Amazon Security Lake is located.

**Account ID**: The ID of the AWS Account whose admin enabled Amazon Security Lake.

**Custom source name**: The name of the custom source you [created in Amazon Security Lake](usecase-security-lake#create-custom-source) to receive data from this Destination.

**Staging location**: Filesystem location in which to locally buffer files before compressing and moving to final destination. Cribl recommends that this location be stable and high-performance.

> The **Staging location** field is not displayed or available on Cribl.Cloud-managed Worker Nodes.
>
{.box .cloud}

### Optional Settings

**File name prefix expression**: The output filename prefix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `CriblOut`.

**Backpressure behavior**: Select whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`.

**Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Stream's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Authentication

For the **Authentication Method**, you **must** leave the default **Auto** method selected.

This method uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance. The attached IAM role grants Cribl Stream Worker Nodes access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

### Assume Role

For these settings, you'll enter values that you noted when [creating the custom source](usecase-security-lake#create-custom-source) in Security Lake.

**AssumeRole ARN**: The custom source's **Provider role ARN**.

**External ID**: In the custom source, this is the value of the `ExternalId` element of the trust relationship JSON object.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output. Use the drop-down to select the Pack you [installed](usecase-security-lake#set-up-stream) when setting up Cribl Stream.

**System fields**: Remove any fields specified here.

### Parquet Settings {#parquet-settings} 

Note that:
- Cribl Edge Workers support Parquet only when running on Linux, not on Windows.

**Row group size**: Ideal memory size for row group segments. For example, 128 MB or 1 GB. Affects memory use when writing. Imposes a target, not a strict limit; the final size of a row group may be larger or smaller. Defaults to 16 MB.

**Page size**: Ideal memory size for page segments. For example, 1 MB or 128 MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; the final size of a row group may be larger or smaller. Defaults to 1 MB.

**Parquet version**: Which version of Parquet your schemas are in determines which data types are supported and how they are represented. Defaults to version 2.6.

**Data page version**: Serialization format of data pages. Note that not all reader implementations support Data page V2.

### Advanced Settings

**Max file size (MB)**: Maximum uncompressed output file size. Files of this size will be closed and moved to final output location. Defaults to `32`.

**Max file open time (sec)**: Maximum amount of time to write to a file. Files open for longer than this limit will be closed and moved to final output location. Defaults to `300`.

**Max file idle time (sec)**: Maximum amount of time to keep inactive files open. Files open for longer than this limit will be closed and moved to final output location. Defaults to `30`.

**Max open files**: Maximum number of files to keep open concurrently. When this limit is exceeded, on any individual Worker Node Process, Cribl Stream will close the oldest open files, and move them to the final output location. Defaults to `100`.

> Cribl Stream will close files when **any** of the four above conditions is met.
>
{.box .info}

**Max concurrent file parts**: Maximum number of parts to upload in parallel per file. A value of `1` tells the Destination to send the whole file at once. When set to `2` or above, IAM permissions must include those required for [multipart uploads](#permissions). Defaults to `4`; highest allowed value is `10`.

**Add Output ID**: When set to `Yes` (the default), adds the **Output ID** field's value to the staging location's file path. This ensures that each Destination's logs will write to its own bucket.

**Remove staging dirs**: Toggle to `Yes` to delete empty staging directories after moving files. This prevents the proliferation of orphaned empty directories. When enabled, exposes this additional option:

* **Staging cleanup period**: How often (in seconds) to delete empty directories. Defaults to `300` seconds (every 5 minutes). Minimum configurable interval is `10` seconds; maximum is `86400` seconds (every 24 hours).

**Endpoint**: Security Lake service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to Security Lake-compatible endpoint.

**Object ACL**: Object ACL (Access Control List) to assign to uploaded objects.

**Storage class**: Select a storage class for uploaded objects. Defaults to `Standard`. The other options are: `Reduced Redundancy Storage`; `Standard, Infrequent Access`; `One Zone, Infrequent Access`; `Intelligent Tiering`; `Glacier`; or `Deep Archive`.

**Server-side encryption**: Encryption type for uploaded objects – used to enable encryption on data at rest. Defaults to no encryption; the other options are `Amazon S3 Managed Key` or `AWS KMS Managed Key`. 

> Amazon S3 always encrypts data in transit using HTTPS, with default one-way authentication from server to clients. Two-way authentication is not required to get encryption, and requires clients to possess a certificate.
>
{.box .success}

**Signature version**: Signature version to use for signing Security Lake requests. Defaults to `v4`.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to `Yes`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## IAM Permissions {#permissions}

The following permissions are always needed to write to an Amazon S3 bucket: 

* `s3:ListBucket`
* `s3:GetBucketLocation`
* `s3:PutObject`

If your Destination needs to do multipart uploads to S3, two more permissions are needed:

* `kms:GenerateDataKey`
* `kms:Decrypt`

See the [AWS documentation](https://repost.aws/knowledge-center/s3-large-file-encryption-kms-key).

## Creating and Modifying Parquet Schemas {#creating-parquet-schemas}

When you need to add a new Parquet schema or modify an existing one:

1. Navigate to the Parquet Schema Library, located at **Processing** > **Knowledge** > **Parquet Schemas**.

2. Create or modify the schema, as described [here](parquet-schemas#parquet-schemas).
  > Cribl [strongly recommends](#cloning-is-safer) that to modify an existing schema, you first clone it and give the clone its own distinct name.
  {.box .warning}

3. Return to this Destination, and select the new or modified
schema in the **Parquet schema** drop-down.

4. Click **Save**.

#### Cloning First is Safer Than Just Modifying {#cloning-is-safer}

Modifying an existing schema in the Schema Library does **not** propagate your modifications to the Destination's copy of the schema.
* Cloning and renaming schemas is the safest approach, because in Step 3 above, it removes any doubt that your Destination will now use the newly-modified version of your schema. 
* If you do **not** clone and rename the schema (i.e., you leave the schema name unchanged), you still must re-select the schema in the Destination's **Parquet schema** drop-down, and click **Save**, to bring the modified version into the Destination.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in forwarding data to a Destination. 

Field for this Destination:

- `__partition`

## Troubleshooting Resources {#troubleshooting}

[Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Destination Integrations: S3](https://university.cribl.io/#/online-courses/a29a2a29-f1c2-414a-9d58-74c0af4fb15c) short course. Since Amazon Security Lake relies on S3 buckets, what you learn from the course can augment your Security Lake skills.

To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
