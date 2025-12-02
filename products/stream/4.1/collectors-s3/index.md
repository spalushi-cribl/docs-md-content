# S3



Cribl Stream supports collecting data from [Amazon S3](https://aws.amazon.com/s3/) stores. This page covers how to configure the Collector. 

> For a step-by-step tutorial on using Cribl Stream to replay data from an S3-compatible store, see our [Data Collection & Replay sandbox](https://sandbox.cribl.io/course/data-collection). The sandbox takes about 30 minutes. It provides a hosted environment, with all inputs and outputs preconfigured for you.
> 
> Also see our [Amazon S3 Better Practices](/stream/usecase-s3-better-practices) and [Using S3 Storage and Replay](/stream/usecase-replay-s3) guides.
>
{.box .success}

## How the Collector Pulls Data

When you run an S3 Collector in Discovery mode, the first available Worker returns the list of available files to the Leader Node.

In Full Run mode, the Leader distributes the list of files to process across 1..*N* Workers, as evenly as possible, based on file size. Each Worker then streams the files from the S3 bucket/path to itself.

### Compression

Cribl Stream can ingest compressed S3 files if they meet all the following conditions: 
- Compressed with the `x-gzip` MIME type.
- End with the `.gz` extension.
- Can be uncompressed using the `zlib.gunzip` algorithm. 

### Storage Class Compatibility

Cribl Stream does **not** support data preview, collection, or replay
from S3 Glacier or S3 Deep Glacier storage classes, whose stated retrieval lags
(variously minutes to 48 hours) cannot guarantee data availability when the
Collector needs it.

Cribl Stream **does** support data preview, collection, and replay from S3 Glacier
Instant Retrieval when you're using the S3 Intelligent-Tiering storage class.

## Configuring an S3 Collector 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, select **Data** > **Sources**, then select **Collectors** > **S3** from the **Manage Sources** page's tiles or left nav. Click **Add Collector** to open the **S3** > **New Collector** modal, which provides the following options and fields.

> The sections described below are spread across several tabs. Click the tab links at left to navigate among tabs. Click **Save** when you've configured your Collector.
> 
> Collector Sources currently cannot be selected or enabled in the QuickConnect UI.
>
{.box .info}

### Collector Settings

The Collector Settings determine how data is collected before processing.

**Collector ID**: Unique ID for this Collector. E.g., `Attic42TreasureChest`.

**Auto-populate from**: Select a Destination with which to auto-populate Collector settings. Useful when replaying data.

**S3 bucket**: Simple Storage Service bucket from which to collect data.

### Authentication

Select an AWS authentication method.

The **Auto** option (default) will use environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, or the attached IAM role. Will work only when running on AWS.

The **Manual** option exposes these fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. if not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials. Optional when running on AWS.

The **Secret** option swaps in this drop-down:

- **Secret key pair**: Select a secret key pair that you've [configured](kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. Follow the **Create** link if you need to configure a key pair.

> The **Manual** option's **Access key** and **Secret key** values override the **Auto** option. For any Workers that you want to associate with an AWS IAM role, using the **Assume Role** controls below – including Cribl-managed or hybrid Cribl.Cloud Workers – ensure that the **Manual** option's key fields are empty. 
>
{.box .warning}

### Assume Role

**Enable Assume Role**: Slide to `Yes` to enable Assume Role behavior.

**AssumeRole ARN**: Amazon Resource Name (ARN) of the role to assume.

**External ID**: External ID to use when assuming role. Enter this if defined in your IAM policy's trust relationship; otherwise, leave blank. (Usage example: [AWS Cross-Account Data Collection](/stream/usecase-aws-x-account#account-b-configuration).)

### Optional Settings

**Region**: S3 Region from which to retrieve data.

**Path**: Path, within the bucket, from which to collect data. Templating is supported (e.g., `/myDir/${host}/${year}/${month}/`). More on templates and Filters [here](collectors-schedule-run#tokens).

**Path extractors**: Extractors enable you to use template tokens as context for expressions that enrich discovery results. For usage details and examples, see [Using Path Extractors](#path-extractors).

**Endpoint**: S3 service endpoint. If empty, Cribl Stream will automatically construct the endpoint from the AWS Region. To access the AWS S3 endpoints, use the path-style URL format. You don't need to specify the bucket name in the URL, because Cribl Stream will automatically add it to the URL path. For details, see AWS' [Path‑Style Requests](https://docs.aws.amazon.com/AmazonS3/latest/userguide/VirtualHosting.html#path-style-access) topic.
 
**Signature version**: Signature version to use for signing S3 requests. Defaults to `v4`.

**Recursive**: If set to `Yes` (the default), data collection will recurse through subdirectories. 

**Max batch size (files)**: Maximum number of lines written to the discovery results files each time. Defaults to `10`. To override this limit in the Collector's Schedule/Run modal, use [Advanced Settings > Upper task bundle size](collectors-schedule-run#advanced-settings).



**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`.

**Verify bucket permissions**: Toggle this to `No` if you can access files within the bucket, but not the bucket itself. This resolves errors of the form: `discover task initialization failed...error: Forbidden`.

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Result Settings

The Result Settings determine how Cribl Stream transforms and routes the collected data.

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

**Enabled**: Defaults to `No`. Toggle to `Yes` to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Click **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute the field's value (can be a constant).

#### Result Routing

**Send to Routes**: If set to `Yes` (the default), Cribl Stream will send events to normal routing and event processing. Toggle to `No` to select a specific Pipeline/Destination combination. The `No` setting exposes these two additional fields:
- **Pipeline**:  Select a Pipeline to process results.
- **Destination**: Select a Destination to receive results.

The default `Yes` setting instead exposes this field: 
- **Pre-processing Pipeline**: Pipeline to process results before sending to Routes. Optional.

This field is always exposed:
- **Throttling**: Rate (in bytes per second) to throttle while writing to an output. Also takes values with multiple-byte units, such as `KB`, `MB`, `GB`, etc. (Example: `42 MB`.) Default value of `0` indicates no throttling.

> You might disable **Send to Routes** when configuring a Collector that will connect data from a specific Source to a specific Pipeline and Destination. This keeps the Collector's configuration self‑contained and separate from Cribl Stream's routing table for live data – potentially simplifying the Routes structure.
>
{.box .info}

### Advanced Settings

Advanced Settings enable you to customize post-processing and administrative options.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

**Time to live**: How long to keep the job's artifacts on disk after job completion. This also affects how long a job is listed in **Job Inspector**. Defaults to `4h`.

**Remove Discover fields** : List of fields to remove from the Discover results. This is useful when discovery returns sensitive fields that should not be exposed in the Jobs user interface. You can specify wildcards (such as `aws*`).

**Resume job on boot**: Toggle to `Yes` to resume ad hoc collection jobs if Cribl Stream restarts during the jobs' execution.

### Using Path Extractors {#path-extractors}

Click **Add Extractor** to add each extractor as a key-value pair, mapping a **Token** name on the left (a substring of the **Path**) to a custom JavaScript **Extractor expression** on the right (e.g., `{host: value.toLowerCase()}`). 

Each expression accesses its corresponding token through its `value` variable, and evaluates the token to populate event fields.

> You can define each token in only one **Extractor expression** per S3 Collector config.
>
{.box .warning}

#### Example #1

Here is a simple, complete example, using a single expression:

Token | Extractor Expression | Matched Value | Extracted Result
--- | --- | --- | ---
`/var/log/${foobar}` | `{program: value.split('.')[0]}` | `/var/log/syslog.1`| `{program: syslog, foobar: syslog.1}`

#### Example #2

Here is a multi-expression example. Assuming that your S3 bucket path supports the Collector's configured **Path**, this example shows how to extract all segments of this example **Path** and transform each one into a reformatted field.

First, the path:

`/${index}/${_time:%Y}/${_time:%m}/${_time:%d}/${host}/${category}/${_time:%H}/${file}`

Next, the tokens and extractor expressions:

Token | Extractor Expression | Purpose
--- | --- | ---
`index` | `{"indexUppercase" : value.toUpperCase()}` | Uppercase the `index`
`host`  | `{"hostUpperPrefix" : value.toUpperCase().replace(/(.+?)\..+/,'$1'), "domain" : value.match(/.+?\.(.+)/)[1]}` | Use one token for two fields: `hostUpperPrefix` and `domain`
`category` | ``{"categoryUpperFirstLetter" : `${value.substring(0,1).toUpperCase()}${value.substring(1)}`} `` | Uppercase the `category` string's first letter
`file`  | `{"fileCleanName" : value.replace(/ARCHIVE-/i,'').toUpperCase().split('.')[0], "CRIBL_WORKER_ID" : value.split('.')[1], "compression" : value.split('.')[2]}` | Split the object (file name) into three fields

Here is a corresponding example event, as collected/replayed from S3. The metadata fields at the bottom show the results of the path extractor expressions:

![Event with path extractors' transformations](path-extractor-s3-result.6b275abba9.png)
{scale="80%" border="true"}



## S3 Permissions

The following permissions are required on the S3 bucket:

* `s3:GetObject`
* `s3:ListBucket`
* `s3:GetBucketLocation` – Needed if **Region** is not configured.

## Temporary Access via SSO Provider

You can use [Okta](usecase-sso-okta#temp-access) or [SAML](usecase-sso-saml#temp-access) to provide access to S3 buckets using temporary security credentials.

## Troubleshooting

When permissions are correct on the object store, and events are reaching the Collector, the Preview pane will show events and the Job Inspector will show an **Events collected** count. 

However, if previewing returns no events and throws no error, first check your Filter expression by previewing without it (e.g., simplify the Filter expression to `true`). Then check the Job Inspector: If the **Total size** is greater than `0`, and the **Received size** is `NA` or `0`, make sure you have list and read permissions on the object store.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).




