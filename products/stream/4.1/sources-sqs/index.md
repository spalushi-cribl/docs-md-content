# Amazon SQS


Cribl Stream supports receiving events from [Amazon Simple Queuing Service](https://aws.amazon.com/sqs/).

> Type: **Pull**  |  TLS Support: **YES** (secure API) | Event Breaker Support: **No**
>
{.box .info}

## Configuring Cribl Stream to Receive Data from Amazon SQS 

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Pull** > ] **Amazon** > **SQS**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Pull** > ] **Amazon** > **SQS**. Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this SQS Source definition. 

**Queue**: The name, URL, or ARN of the SQS queue to read events from. This value must be a JavaScript expression (which can evaluate to a constant), enclosed in single quotes, double quotes, or backticks. To specify a non-AWS URL, use the format: `'{url}/<queueName>'`. (E.g., `':port/<myQueueName>'`.)

**Queue type**: The queue type used (or created). Defaults to `Standard`. `FIFO` (First In, First Out) is the other option.

### Optional Settings

**Create queue**: If toggled to `Yes`, Cribl Stream will create the queue if it does not exist.

**Region**: AWS Region where the SQS queue is located. Required, unless the **Queue** entry is a URL or ARN that includes a Region. 

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Authentication

Use the **Authentication Method** buttons to select an AWS authentication method.

###### Auto

This default option uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance, local credentials, sidecar, or other source. The attached IAM role grants Cribl Stream Workers access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

###### Manual

If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, e.g., those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

###### Secret

If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select a secret key pair that you've [configured](kms-config) in Cribl Stream's internal secrets manager or (if enabled) an external KMS. Follow the **Create** link if you need to configure a key pair.

### Assume Role

**Enable for SQS**: Whether to use Assume Role credentials to access SQS. Defaults to `No`.

**AWS account ID**: SQS queue owner's AWS account ID. Leave empty if SQS queue is in same AWS account.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the external ID to use when assuming role.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event, using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Endpoint**: SQS service endpoint. If empty, the endpoint will be automatically constructed from the AWS Region.

**Signature version**: Signature version to use for signing SQS requests. Defaults to `v4`; `v2` is also available.

**Max messages**: The maximum number of messages that SQS should return in a poll request. Amazon SQS never returns more messages than this value. (However, fewer messages might be returned.) Acceptable values: `1` to `10`. Defaults to `10`. 

**Visibility timeout seconds**: The duration (in seconds) that the received messages are hidden from subsequent retrieve requests, after they're retrieved by a `ReceiveMessage` request. Defaults to `600`.



**Num receivers**: The number of receiver processes to run. The higher the number, the better the throughput, at the expense of CPU overhead. Defaults to `3`.

**Poll timeout (secs)**: How long to wait for events before polling again. Minimum `1` second; default `10`; maximum `20`. Short durations increase the number ‑ and thus the cost – of requests sent to AWS. (The UI will show a warning for intervals shorter than `5` seconds.) Long durations increase the time the Source takes to react to configuration changes and system restarts.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to reject certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`, the restrictive option.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__final`
* `__inputId`
* `__raw`
* `__receivedTs`
* `__sqsMessageId`
* `__sqsReceiptHandle`
* `_time`

## SQS Permissions

The following permissions are needed on the SQS queue:

  * `sqs:ReceiveMessage`
  * `sqs:DeleteMessage`
  * `sqs:GetQueueAttributes`
  * `sqs:GetQueueUrl`
  * `sqs:CreateQueue`  (optional, if and only if you want Cribl Stream to create the queue)

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting Notes

> VPC [endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) for [SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-vpc-endpoints.html) might need to be set up in your account. Check with your administrator for details.
>
{.box .warning}

## How Cribl Stream Pulls Data

Workers poll messages from SQS. The call will return a message if one is available, or will time out after 1 second if no messages are available. 

Each Worker gets its share of the load from SQS, and it receives a notification of a file newly added to an S3 bucket. By default, SQS returns a maximum of 10 messages in a single poll request.
