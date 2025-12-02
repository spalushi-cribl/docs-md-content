# Amazon Security Lake Destination


This Destination delivers data to [Amazon Security Lake](https://docs.aws.amazon.com/security-lake/latest/userguide/what-is-security-lake.html), a fully-managed security data lake service backed by Amazon S3 buckets.

> Type: Non-Streaming | TLS Support: Yes | PQ Support: No
>
> This Destination is unavailable on Windows and macOS.
>
{.box .info}

Security Lake ingests events described by the [Open Cybersecurity Schema Framework](https://schema.ocsf.io/categories) (OCSF) and conveyed as Parquet files. This Destination sends the Parquet files in batches. Cribl Edge does not read or replay the data sent to Security Lake.

## A Complete Setup Scenario

Cribl recommends that you start with the [Amazon Security Lake Integration](usecase-security-lake) topic, which will guide you through the following steps in the correct order:
 
1. Set up Amazon Security Lake.
1. Set up Cribl Stream.
1. Create an Amazon Security Lake [custom source](https://docs.aws.amazon.com/security-lake/latest/userguide/custom-sources.html), This is an S3 bucket dedicated to a single OCSF event class.
1. Create a Cribl Amazon Security Lake Destination.
1. Connect a Cribl Source to the Amazon Security Lake Destination.
1. Send data to Amazon Security Lake.
 
 As outlined above, you'll create and configure this Destination as the fourth of the six overall steps. The Integration topic covers the key points about configuring this Destination. Refer back to this reference topic, as needed, for details about any given setting. Some of the instructions below assume that you have recorded IDs, values, etc., from procedures you've already completed according to the Integration topic.

> Although Cribl Edge does **not** need to be deployed in [Cribl.Cloud](deploy-cloud) in order to deliver data to Amazon Security Lake, setup procedures on Cribl>Cloud will differ from those documented here. To learn more, please contact Cribl via the `#packs` channel of Cribl Community Slack.
>
{.box .cloud}

## Configure Cribl Edge to Output to Amazon Security Lake 

In Cribl Edge, configure Amazon Security Lake:  

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   -**Output ID**: Enter a unique name to identify this Amazon Security Lake Destination definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **S3 bucket name**: Name of the destination S3 Bucket you created when [setting up Amazon Security Lake](usecase-security-lake#set-up-security-lake). This value can be a constant, or a JavaScript expression that will be evaluated only at init time. For example, referencing a Global Variable: `myBucket-${C.vars.myVar}`.
      > Event-level variables are not available for JavaScript expressions. This is because the bucket name is evaluated only at Destination initialization.
      >
      {.box .info}
   - **Region**: Region where the Amazon Security Lake is located.
   - **Account ID**: The ID of the AWS Account whose admin enabled Amazon Security Lake.
   - **Custom source name**: The name of the custom source you [created in Amazon Security Lake](usecase-security-lake#create-custom-source) to receive data from this Destination.
   - **Staging location**: Filesystem location in which to locally buffer files before compressing and moving to final destination. Cribl recommends that this location be stable and high-performance.

      > The **Staging location** field is not displayed or available on Cribl.Cloud-managed Edge Nodes.
      >
      {.box .cloud}
3. Next, you can configure the following **Optional Settings**: 
     - **File name prefix expression**: The output filename prefix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `CriblOut`.
     - **Backpressure behavior**: Select whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`. 
     - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Authentication](#auth), [Assume Role](#assume), [Processing](#processing), [Parquet](#parquet-settings), and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 
   
### Authentication {#auth}

For the **Authentication Method**, you **must** leave the default **Auto** method selected.

This method uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance. The attached IAM role grants Cribl Edge Edge Nodes access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

### Assume Role {#assume}

For these settings, you'll enter values that you noted when [creating the custom source](usecase-security-lake#create-custom-source) in Security Lake.

**AssumeRole ARN**: The custom source's **Provider role ARN**.

**External ID**: In the custom source, this is the value of the `ExternalId` element of the trust relationship JSON object.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output. Use the drop-down to select the Pack you [installed](usecase-security-lake#set-up-stream) when setting up Cribl Edge.

**System fields**: Remove any fields specified here.

### Parquet Settings {#parquet-settings} 

**Parquet schema**: Select a schema from the drop-down.

> If you need to modify a schema or add a new one, follow the instructions in our [Parquet Schemas](parquet-schemas#creating-parquet-schemas) topic. These steps will propagate the freshest schema back to this drop-down.
>
{.box .warning}


**Row group size**: Ideal memory size for row group segments. For example, 128 MB or 1 GB. Affects memory use when writing. Imposes a target, not a strict limit; the final size of a row group may be larger or smaller. Defaults to 16 MB.

**Page size**: Ideal memory size for page segments. For example, 1 MB or 128 MB. Generally, lower values improve reading speed, while higher values improve compression. Imposes a target, not a strict limit; the final size of a row group may be larger or smaller. Defaults to 1 MB.

**Parquet version**: Which version of Parquet your schemas are in determines which data types are supported and how they are represented. Defaults to version 2.6.

**Data page version**: Serialization format of data pages. Note that not all reader implementations support Data page V2.

> Cribl Edge Workers support Parquet only when running on Linux, not on Windows.
>
{.box .info}

### Advanced Settings {#advanced-tab}

**File size limit (MB)**: Maximum uncompressed output file size. Files of this size will be closed and moved to final output location. Defaults to `32`.

**File open time limit (sec)**: Maximum amount of time to write to a file. Files open for longer than this limit will be closed and moved to final output location. Defaults to `300`.

**Idle time limit (sec)**: Maximum amount of time to keep inactive files open. Files open for longer than this limit will be closed and moved to final output location. Defaults to `30`.

**Open file limit**: Maximum number of files to keep open concurrently. When this limit is exceeded, on any individual Edge Node Process, Cribl Edge will close the oldest open files, and move them to the final output location. Defaults to `100`.

> Cribl Edge will close files when **any** of the four above conditions is met.
>
{.box .info}

**Concurrent file parts limit**: Maximum number of parts to upload in parallel per file. A value of `1` tells the Destination to send the whole file at once. When set to `2` or above, IAM permissions must include those required for [multipart uploads](#permissions). Defaults to `4`; highest allowed value is `10`.

**Add Output ID**: When toggled on (default), adds the **Output ID** field's value to the staging location's file path. This ensures that each Destination's logs will write to its own bucket.

**Remove staging dirs**: Toggle this on to delete empty staging directories after moving files. This prevents the proliferation of orphaned empty directories. When enabled, exposes this additional option:

* **Staging cleanup period**: How often (in seconds) to delete empty directories. Defaults to `300` seconds (every 5 minutes). Minimum configurable interval is `10` seconds; maximum is `86400` seconds (every 24 hours).

**Enable dead-lettering**: Toggle this on to set a maximum number of retries, and to move files to a designated directory when write failures exceed that limit. This prevents data flow blockage and excessive error logging due to undeliverable files. When enabled, exposes two additional fields:
   - **Dead-letter location**: Specify the storage location for undeliverable files. Defaults to `$CRIBL_HOME/state/outputs/dead-letter`.
   - **Maximum retry limit**: Configure the retry limit for failed file deliveries. This setting defines how many times the system will attempt to move a file to its intended location before it is deemed undeliverable and placed in the dead-letter directory. Defaults to `20`.

**Endpoint**: Security Lake service endpoint. If empty, defaults to AWS' Region-specific endpoint. Otherwise, it must point to Security Lake-compatible endpoint.

**Object ACL**: Object ACL (Access Control List) to assign to uploaded objects.

**Storage class**: Select a storage class for uploaded objects. Defaults to `Standard`. The other options are: `Reduced Redundancy Storage`; `Standard, Infrequent Access`; `One Zone, Infrequent Access`; `Intelligent Tiering`; `Glacier`; or `Deep Archive`.

**Server-side encryption**: Encryption type for uploaded objects – used to enable encryption on data at rest. Defaults to no encryption; the other options are `Amazon S3 Managed Key` or `AWS KMS Managed Key`. 

> Amazon S3 always encrypts data in transit using HTTPS, with default one-way authentication from server to clients. Two-way authentication is not required to get encryption, and requires clients to possess a certificate.
>
{.box .success}

**Signature version**: Signature version to use for signing Security Lake requests. Defaults to `v4`.

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on.

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

Cribl Edge uses a set of internal fields to assist in forwarding data to a Destination. 

Field for this Destination:

- `__partition`

## Troubleshooting Resources {#troubleshooting}

> [Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Destination Integrations: S3](https://university.cribl.io/#/online-courses/a29a2a29-f1c2-414a-9d58-74c0af4fb15c) short course. Since Amazon Security Lake relies on S3 buckets, what you learn from the course can augment your Security Lake skills.
> <br>To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. <br>
>You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) <br> Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
{.box .course}
