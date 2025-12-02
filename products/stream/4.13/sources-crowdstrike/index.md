# CrowdStrike FDR Source


Cribl Stream supports receiving data from the [CrowdStrike](https://www.crowdstrike.com/resources/guides/) Falcon Data Replicator (FDR) platform. CrowdStrike data can then be sent to SIEM, threat-hunting, and other security tools and platforms. 

>   Type: **Pull**  |  TLS Support: **Yes** | Event Breaker Support: **Yes**
>
{.box .info}

This page covers how to configure the Source. Because the FDR platform pulls data from Amazon S3 buckets maintained by CrowdStrike, some of the configuration described here actually involves S3.  

For configuration examples that you can import and adapt to your needs, examine these resources on the Cribl Packs Dispensary:

- [CrowdStrike FDR Pack](https://packs.cribl.io/packs/cribl_crowdstrike).

- [Exabeam Pack for CrowdStrike FDR](https://packs.cribl.io/packs/cribl_exabeam_crowdstrike_FDR).

## Configure a CrowdStrike Source

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect**. Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**. 
     - To configure via the [Routes](routes), select **Data** > **Sources**. Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
     - **Input ID**: Unique ID for this Source. For example, `Endpoint42Investigation`.
     - **Description**: Optionally, enter a description.
     - **Queue**: The name, URL, or ARN of the SQS queue to read notifications from. When a non-AWS URL is specified, format must be: `'{url}/myQueueName'`. For example, `'https://host:port/myQueueName'`. The value must be a JavaScript expression (which can evaluate to a constant value), enclosed in quotes or backticks. The expression can only be evaluated at init time, for example when referencing a Global Variable like this: `https://host:port/myQueue-${C.vars.myVar}`.
3. Next, you can configure the following **Optional Settings**: 
    - **Filename filter**: Regex matching file names to download and process. Defaults to: `.*`.
    -  **Region**: AWS Region where the S3 bucket and SQS queue are located. Required, unless **Queue** is a URL or ARN that includes a Region.
    -  **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Authentication](#auth), [Assume Role](#assume), [Processing](#processing), and [Advanced](#advanced-tab) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 


### Authentication {#auth}

Use the buttons to select an authentication method.

[Snippet not found: content/shared/4.13/snippets/_integrations-aws-auto-auth.md]

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, for example, those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select a secret key pair that you've [configured](securing-kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. Follow the **Create** link if you need to configure a key pair.

### Assume Role {#assume}

When using Assume Role to access resources in a different Region than Cribl Stream, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) (STS) endpoint specific to that Region by using the `CRIBL_AWS_STS_REGION` environment variable on your Worker Node. Setting an invalid Region results in a fallback to the global STS endpoint.

**Enable for Amazon S3**: Whether to use Assume Role credentials to access S3. Defaults to toggled on.

**Enable for Amazon SQS**: Whether to use Assume Role credentials when accessing SQS (Amazon Simple Queue Service). Default is toggled off.

**AWS account ID**: SQS queue owner's AWS account ID. Leave empty if the SQS queue is in the same AWS account.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

### Processing Settings {#processing}

#### Custom Command

In this section, you can pass the data from this input to an external command for processing, before the data continues downstream.

**Enabled**: Default is toggled off. Toggle on to enable the custom command.

**Command**: Enter the command that will consume the data (via `stdin`) and will process its output (via `stdout`).

**Arguments**: Click **Add Argument** to add each argument to the command. You can drag arguments vertically to resequence them.

#### Event Breakers

In this section, you can apply event breaking rules to convert data streams to discrete events.

**Event Breaker rulesets**: A list of event breaking rulesets that will be applied, in order, to the input data stream. Defaults to `System Default Rule`.

**Event Breaker buffer timeout**: How long (in milliseconds) the Event Breaker will wait for new data to be sent to a specific channel, before flushing out the data stream, as-is, to the Routes. Minimum `10` ms, default `10000` (10 sec), maximum `43200000` (12 hours).

#### Fields 

[Snippet not found: content/shared/4.13/snippets/_sources-add-fields.md]

#### Pre-Processing

Optionally, use the drop-down to select an existing [Pipeline](pipelines) to process data from this Source before sending it through the Routes. Otherwise (by default), events will be sent to normal routing and event processing. 

### Advanced Settings {#advanced-tab}

Advanced Settings enable you to customize post-processing and administrative options.

#### Checkpoint Settings

**Enable checkpointing**: When toggled off (default) Cribl Stream will always start processing a file from the beginning. Toggle on if, after an error or restart occurs, you want Cribl Stream to resume processing a given file from the point where it was interrupted, as explained [below](#checkpointing). When turned on, the following additional setting becomes available:

* **Retries**: The number of times you want Cribl Stream to retry processing a file after an error or restart occurs. If **Skip file on error** is enabled, this setting is ignored.

#### Other Settings

**Endpoint**: The S3 service endpoint you want CrowdStrike to use. If empty, Cribl Stream will automatically construct the endpoint from the AWS Region. To access the AWS S3 endpoints, use the path-style URL format. You don't need to specify the bucket name in the URL, because Cribl Stream will automatically add it to the URL path. For details, see AWS' [Path‑Style Requests](https://docs.aws.amazon.com/AmazonS3/latest/userguide/VirtualHosting.html#path-style-access) topic.
 
**Signature version**: Signature version to use for signing S3 requests. Defaults to `v4`; `v2` is also available.

**Message limit**: The maximum number of messages that SQS should return in a poll request. Amazon SQS never returns more messages than this value. (However, fewer messages might be returned.) Acceptable values: `1` to `10`. Defaults to `1`. 

**Visibility timeout seconds**: After messages are retrieved by a `ReceiveMessage` request, Cribl Stream will hide those messages from subsequent retrieve requests for at least this duration. Default is `21600` seconds (6 hours); maximum allowed threshold is `43200` sec. (12 hours). Set to `0` for no message hiding.

> Cribl Stream will automatically extend this timeout until the initial request's files have been processed – notably, for large files that require additional processing time. The generous 6‑hour default allows each Worker Process time to decompress, stitch, and read large multi-part files, without duplicate processing.
>
{.box .info}

**Number of receivers**: The number of receiver processes to run. The higher the number, the better the throughput, at the expense of CPU overhead. Defaults to `1`.

**Poll timeout (secs)**: How long to wait for events before polling again. Minimum `1` second; default `10`; maximum `20`. Short durations increase the number ‑ and thus the cost – of requests sent to AWS. (The UI will show a warning for intervals shorter than `5` seconds.) Long durations increase the time the Source takes to react to configuration changes and system restarts.

**Socket timeout**: Socket inactivity timeout (in seconds). Increase this value if retrievals time out during backpressure. Defaults to `300` seconds.

**Skip file on error**: Toggle on to skip files that trigger a processing error. (For example, corrupted files.) Default is toggled off, which enables retries after a processing error.

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to reject certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on, the restrictive option.

**Encoding**: Character encoding to use when parsing ingested data. If not set, Cribl Stream will default to UTF-8 but might incorrectly interpret multi-byte characters. This option is ignored for Parquet files. UTF-16LE and Latin-1 are also supported.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Checkpointing {#checkpointing}

When Workers restart, checkpointing can prevent the Source from losing data or collecting duplicate events. Among the situations where this works well: 

* When Cribl Stream pushes a new configuration: the Leader pushes the configuration to the Workers, and restarts them.
* When a Worker has gotten into a bad state, and you want to [restart](configuration-files#restart) it.

Checkpointing allows Cribl Stream to resume processing a file at the point where an error or restart occurred, minimizing the number of duplicate events sent downstream in the event of an interruption. To accomplish this, Cribl Stream keeps track of how many events have been processed for a given file. Resuming processing after an interruption, the Source skips the first *N* events it has seen and starts processing from event *N*+1.

The Source manages checkpoint information in memory, but writes it out to disk periodically (once per second, and on Worker shutdown) so that the information can survive a Worker restart. Cribl Stream keeps checkpoint information at the Worker Process level and does not share it between Worker Processes or Worker Nodes.

Checkpointing is a feature to keep in mind when you do disaster recovery (DR) planning.

Caveats to using checkpointing include: Whenever Worker Node state is destroyed (that is, when a Worker ceases to exist), checkpoint information will be lost. In Cribl.Cloud deployments, this can happen during upgrades. Also, because checkpoints are not written to disk on **every** event, the checkpoints written to disk will lag behind those that the S3 Source manages in memory. If the Worker Process stops abruptly without a clean shutdown, checkpoints not yet written to disk will be lost. When processing resumes, the Source will start from just after the last checkpoint written to disk, producing duplicate events corresponding to the lost checkpoints.

Located in the UI at **Advanced Settings** > **Checkpoint Settings** > **Enable checkpointing**, this feature is turned off by default.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## How Cribl Stream Pulls Data

Worker Processes poll for messages from SQS, using the AWS SDK S3 APIs. Each polling call will return (by default) one message, if any are available. You can change this default using `Max messages`. 

If no message is available, the call will time out after one second, and then polling will repeat. All Worker Processes participate in polling, so as to disribute files' collection and processing evenly across Workers.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting

[Snippet not found: content/shared/4.13/snippets/_sources-troubleshooting.md]
