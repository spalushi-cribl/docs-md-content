# Amazon SQS


Cribl Stream supports sending events to [Amazon Simple Queuing Service](https://aws.amazon.com/sqs/). 

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info} 

## Configuring Cribl Stream to Send Data to Amazon SQS 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Destination** at right. From the resulting drawer's tiles, select **Amazon** > **SQS**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.

 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Amazon** > **SQS**. Next, click **New Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this SQS Destination.

**Queue name**: The name, URL, or ARN of the SQS queue to send events to. This value must be a JavaScript expression (which can evaluate to a constant), enclosed in single quotes, double quotes, or backticks. To specify a non-AWS URL, use the format: `'{url}/<queueName>'`. (E.g., `':port/<myQueueName>'`.)

**Queue type**: The queue type used (or created). Defaults to `Standard`. `FIFO` (First In, First Out) is the other option.

### Optional Settings

**Message group ID**: This parameter applies only to queues of type FIFO. Enter the tag that specifies that a message belongs to a specific message group. (Messages belonging to the same message group are processed in FIFO order.) Defaults to `cribl`. Use event field `__messageGroupId` to override this value.

**Create queue**: Specifies whether to create the queue if it does not exist. Defaults to `Yes`.

**Region**: Region where SQS queue is located.

**Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.

**Tags**: Optionally, add tags that you can use for filtering and grouping at the final destination. Use a tab or hard return between (arbitrary) tag names.

### Persistent Queue Settings

> This tab is displayed when the **Backpressure behavior** is set to **Persistent Queue**. 
> 
> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Clear Persistent Queue** button. A maximum queue size of 1 GB disk space is automatically allocated per Worker Process.
>
{.box .info}

**Max file size**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Max queue size**: The maximum amount of disk space the queue is allowed to consume. Once this limit is reached, Cribl Stream stops queueing and applies the fallback **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Stream will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

**Queue-full behavior**: Whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear persistent queue**: Click this button if you want to flush out files that are currently queued for delivery to this Destination. A confirmation modal will appear. (Appears only after **Output ID** has been defined.)

### Authentication

Use the **Authentication Method** buttons to select an AWS authentication method.

**Auto**: This default option uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance. The attached IAM role grants Cribl Stream Workers access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, e.g., those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select an API key/secret key pair that you've [configured](/stream/securing-and-monitoring#secrets) in Cribl Stream's secrets manager. A **Create** link is available to store a new, reusable secret.

### Assume Role

**Enable for SQS**: Toggle to `Yes` to use Assume Role credentials to access SQS.

**AWS account ID**: Enter the SQS queue owner's AWS account ID. Leave empty if the SQS queue is in the same AWS account where this Cribl Stream instance is located.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.

### Advanced Settings

**Endpoint**: SQS service endpoint. If empty, the endpoint will be automatically constructed from the region.

**Signature version**: Signature version to use for signing SQS requests. Defaults to `v4`.

**Max queue size**: Maximum number of queued batches before blocking. Defaults to `100`.

**Max record size (KB)**: Maximum size of each individual record. Per the SQS spec, the maximum allowed value is 256 KB. (the default).

**Flush period (sec)**: Maximum time between requests. Low settings could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Max concurrent requests**: The maximum number of in-progress API requests before backpressure is applied. Defaults to `10`.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## SQS Permissions

The following permissions are needed to write to an SQS queue: 

  * `sqs:ListQueues`
  * `sqs:SendMessage`
  * `sqs:SendMessageBatch`
  * `sqs:CreateQueue`
  * `sqs:GetQueueAttributes`
  * `sqs:SetQueueAttributes`
  * `sqs:GetQueueUrl`

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [functions](functions) can use them to make processing decisions.

Fields for this Destination:

  * `__messageGroupId `
  * `__sqsMsgAttrs`
  * `__sqsSysAttrs`
