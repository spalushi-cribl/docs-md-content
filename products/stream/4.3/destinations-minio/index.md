# MinIO


**MinIO** is a non-streaming Destination type, to which Cribl Stream can output objects.

> Type: Non-Streaming | TLS Support: Configurable | PQ Support: No
>
{.box .info} 

## Configuring Cribl Stream to Output to MinIO Destinations

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Destination** at right. From the resulting drawer's tiles, select **MinIO**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **MinIO**. Next, click **Add Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this MinIO definition.

**MinIO endpoint**: MinIO service URL (e.g., http://minioHost:9000).

**MinIO bucket name**:Name of the destination MinIO bucket. This value can be a constant, or a JavaScript expression that will be evaluated only at init time. E.g., referencing a Global Variable: `myBucket-${C.vars.myVar}`. Ensure that the bucket already exists, otherwise MinIO will generate "bucket does not exist" errors.

> Event-level variables are not available for JavaScript expressions. This is because the bucket name is evaluated only at Destination initialization. If you want to use event-level variables in file paths, Cribl recommends specifying them in the **Partitioning Expression** field (described below), because this is evaluated for each file.
>
{.box .info}

**Staging location**: Filesystem location in which to locally buffer files before compressing and moving to final destination. Cribl recommends that this location be stable and high-performance.

> The **Staging location** field is not displayed or available on Cribl.Cloud-managed Worker Nodes.
>
{.box .cloud}

**Key prefix**: Root directory to prepend to path before uploading. Enter a constant, or a JS expression enclosed in single quotes, double quotes, or backticks.

Prefix to apply to files/objects before uploading to the specified bucket. MinIO will display key prefixes as folders.

**Data format**: The output data format defaults to `JSON`. `Raw` and `Parquet` are also available. Selecting `Parquet` (supported only on Linux, not Windows) exposes a [**Parquet Settings**](#parquet-settings) left tab, where you **must** configure certain options in order to export data in Parquet format.

### Optional Settings

**Partitioning expression**: JavaScript expression that defines how files are partitioned and organized. Default is date-based. If blank, Cribl Stream will fall back to the event's `__partition` field value (if present); or otherwise to the root directory of the **Output Location** and  **Staging Location**.

> Cribl Stream's internal `__partition` field can be populated in multiple ways. The precedence order is: explicit **Partitioning expression** value ‑> ``${host}/${sourcetype}`` (default) **Partitioning expression** value ‑> user-defined `event.__partition`, set with an [Eval](https://docs.cribl.io/docs/eval-function) Function (takes effect only where this **Partitioning expression** field is blank).
>
{.box .info}

**Compress**: Data compression format used before moving to final destination. Defaults to `gzip` (recommended). This setting is not available when **Data format** is set to `Parquet`.

**File name prefix expression**: The output filename prefix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `CriblOut`.

**File name suffix expression**: The output filename suffix. Must be a JavaScript expression (which can evaluate to a constant), enclosed in quotes or backticks. Defaults to `` `.${C.env["CRIBL_WORKER_ID"]}.${__format}${__compression === "gzip" ? ".gz" : ""}` ``, where `__format` can be `json` or `raw`, and `__compression` can be `none` or `gzip`.

**Backpressure behavior**: Select whether to block or drop events when all receivers are exerting backpressure. (Causes might include an accumulation of too many files needing to be closed.) Defaults to `Block`.

**Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Stream's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

#### How MinIO Composes File Names {#paths}

The full path to a file consists of:

`<bucket_name>/<keyprefix><partition_expression |
__partition><file_name_prefix><filename>.<extension>`

As an example, assume that the **MinIO bucket name** is `bucket1`, the **Key prefix** is `aws`, the **Partitioning expression** is `` `${host}/${sourcetype}` ``, the source is undefined, the **File name prefix** is the default `CriblOut`, and the **Data format** is `json`. Here, the full path as displayed in MinIO would have this form: `/bucket1/aws/192.168.1.241/undefined/CriblOut-<randomstring>0.json`

> Although MinIO will display the **Key prefix** and **Partitioning expression** values as folders, both are actually just part of the overall key name, along with the file name.
>
{.box .info}

### Authentication

Use the **Authentication Method** buttons to select one of these options:

**Auto**: This default option uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance, local credentials, sidecar, or other source. The attached IAM role grants Cribl Stream Workers access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, e.g., those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

The values for **Access key** and **Secret key** can be a constant, or a JavaScript expression (such as `${C.env.MY_VAR}`) enclosed in quotes or backticks, which allows configuration with environment variables.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. This option exposes a **Secret key pair** drop-down, in which you can select a [stored secret](/stream/securing-and-monitoring#secrets) that references the set of user-associated IAM credentials described above. A **Create** link is available to store a new, reusable secret.

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Parquet Settings {#parquet-settings} 

To write out Parquet files, note that:
- Cribl Edge Workers support Parquet only when running on Linux, not on Windows.
- See [Working with Parquet](parquet-schemas) for pointers on how to avoid problems such as data mismatches.

**Parquet schema**: Select a schema from the drop-down. 

> If you need to modify a schema or add a new one, follow the instructions in our [Parquet Schemas](parquet-schemas#creating-parquet-schemas) topic. These steps will propagate the freshest schema back to this drop-down.
>
{.box .warning}


**Row group size**: Set the target memory size for row group segments. Modify this value to optimize memory use when writing. Value must be a positive integer smaller than the **File size** value, with appropriate units. Defaults to `16 MB`. 

**Page size**: Set the target memory size for page segments. Generally, set lower values to improve reading speed, or set higher values to improve compression. Value must be a positive integer smaller than the **Row group size** value, with appropriate units. Defaults to `1 MB`.

**Parquet version**: Determines which data types are supported, and how they are represented. Defaults to `2.6`; `2.4` and `1.0` are also available.

**Data page version**: Serialization format for data pages. Defaults to `V2`. If your toolchain includes a Parquet reader that does not support `V2`, use `V1`.

**Log invalid rows**: Toggle to `Yes` to output up to 20 unique rows that were skipped due to data format mismatch. Log level must be set to `debug` for output to be visible.

### Advanced Settings

**Max file size (MB)**: Maximum uncompressed output file size. Files of this size will be closed and moved to final output location. Defaults to `32`.

**Max file open time (sec)**: Maximum amount of time to write to a file. Files open for longer than this limit will be closed and moved to final output location. Defaults to `300`. 

**Max file idle time (sec)**: Maximum amount of time to keep inactive files open. Files open for longer than this limit will be closed and moved to final output location. Defaults to `30`.

**Max open files**: Maximum number of files to keep open concurrently. When exceeded, the oldest open files will be closed and moved to final output location. Defaults to `100`.

> Cribl Stream will close files when **either** of the `Max file size (MB)` or the 
> `Max file open time (sec)` conditions is met.
>
{.box .info}

**Max concurrent file parts**: Maximum number of parts to upload in parallel per file. A value of `1` tells the Destination to send the whole file at once. When set to `2` or above, IAM permissions must include those required for [multipart uploads](#permissions). Defaults to `4`; highest allowed value is `10`.

**Add Output ID**: When set to `Yes` (the default), adds the **Output ID** field's value to the staging location's file path. This ensures that each Destination's logs will write to its own bucket.

> For a Destination originally configured in a Cribl Stream version below 2.4.0, the **Add Output ID** behavior will be switched **off** on the backend, regardless of this toggle's state. This is to avoid losing any files pending in the original staging directory, upon Cribl Stream upgrade and restart. To enable this option for such Destinations, Cribl's recommended migration path is:
> 
> - Clone the Destination.
> - Redirect the Routes referencing the original Destination to instead reference the new, cloned Destination. 
> 
> This way, the original Destination will process pending files (after an idle timeout), and the new, cloned Destination will process newly arriving events with **Add output ID** enabled.
>
{.box .warning}

**Remove staging dirs**: Toggle to `Yes` to delete empty staging directories after moving files. This prevents the proliferation of orphaned empty directories. When enabled, exposes this additional option:
  - **Staging cleanup period**: How often (in seconds) to delete empty directories when **Remove staging dirs** is enabled. Defaults to `300` seconds (every 5 minutes). Minimum configurable interval is `10` seconds; maximum is `86400` seconds (every 24 hours).

**Region**: Region where the MinIO service/cluster is located. Leave blank when using a containerized MinIO.

**Object ACL**: ACL (Access Control List) to assign to uploaded objects. Defaults to `Private`.

**Storage class**: Select a storage class for uploaded objects. Defaults to `Standard`.

**Server-side encryption**: Server side encryption type for uploaded objects. Defaults to `none`.

**Signature version**: Signature version to use for signing MinIO requests. Defaults to `v4`.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## IAM Permissions {#permissions}

The following permissions are always needed to write to an Amazon S3-compatible object store: 

* `s3:ListBucket`
* `s3:GetBucketLocation`
* `s3:PutObject`

If your Destination needs to do multipart uploads to S3, two more permissions are needed:

* `kms:GenerateDataKey`
* `kms:Decrypt`

See the [AWS documentation](https://repost.aws/knowledge-center/s3-large-file-encryption-kms-key).

## Internal Fields

Cribl Stream uses a set of internal fields to assist in forwarding data to a Destination. 

Field for this Destination:

* `__partition`

## Troubleshooting

See also [AWS Sources/​Destinations & S3-Compatible Stores](/stream/common-errors#aws-sources-destinations) for information on common errors.