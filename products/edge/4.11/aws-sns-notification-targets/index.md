# AWS SNS Notification Targets


You can send Notifications to an Amazon Simple Notification Service ([SNS](https://aws.amazon.com/sns/)) 
topic. This gives you access to a broad array of Notification destinations, such
as various AWS services, mobile push notifications, or [text messages](#sms).

To add an Amazon SNS Notification target in Cribl Edge, select **Notifications** in the sidebar, and then select **Targets** > **Add Target** > **AWS SNS**.

## General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in
the **Target ID** column of the **Targets** tab. It can't be changed later, so 
make sure you like it.

## Configuration

**Destination type**: Defaults to **Topic ARN**. The [SMS section](#sms) explains
the **Phone number** option.

**Region**: Select the Region associated with the Amazon S3 bucket.

**Default Topic ARN**: The default Amazon Resource Name ([ARN](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html)) of
the Amazon SNS [topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html) 
to which you want to send Notifications. Cribl Edge expects the ARN in a format
like this: 

`arn:aws:sns:region:account-id:MyTopic`.

If you use a non-AWS URL, the format must be:

`{url}/myQueueName` – for example, `https://host:port/myQueueName`. 

Must be a JavaScript expression (which can evaluate to a constant value),
enclosed in quotes or backticks. Can be evaluated only at initialization time.
For example, if you're referencing a Global Variable:
`https://host:port/myQueue-${C.vars.myVar}`. This value can be overridden by the
Notification event `__topicArn` field.

**Phone number allowlist**: A wildcard list of phone numbers that are allowed to
receive SMS notifications. This is used when **Destination type** is set to
**Phone number**.

## Authentication

**Auto**: This default option uses the AWS instance's metadata service to
automatically obtain short-lived credentials from the IAM role attached to an
EC2 instance, local credentials, sidecar, or other source. The attached IAM role
grants Cribl access to authorized AWS resources. Can also use the environment
variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when
running on AWS.

**Manual**: If not running on AWS, you can select this option to enter a static
set of user-associated IAM credentials (your access key and secret key) directly
or by reference. This is useful for Edge Nodes not in an AWS VPC, like those
running a private cloud. 

The **Manual** option exposes these additional fields:

* **Access key**: Enter your AWS access key. If not present, will fall back to
  the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint
  for IAM role credentials.

* **Secret key**: Enter your AWS secret key. If not present, will fall back to
  the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata
  endpoint for IAM credentials.

The values for **Access key** and **Secret key** can be a constant, or a
JavaScript expression (such as `${C.env.MY_VAR}`) enclosed in quotes or
backticks, which allows configuration with environment variables.

**Secret**: If not running on AWS, you can select this option to supply a stored
secret that references an AWS access key and secret key. The Secret option
exposes this additional field:

* **Secret key pair**: Use the drop-down to select an API key/secret key pair
  that you've configured in Cribl Edge's [secrets manager](securing-secrets). 
  To store a new, reusable secret, select **Create**.

## Assume Role

**Enable for SNS**: Toggle on to define an IAM Role to use, instead of
automatically detecting one locally.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role. This is
required only when assuming a role that requires this ID to delegate
third-party access. For details, see [AWS' documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html).

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds.
Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1
hour).

###  Post‑Processing  {#post-processing}

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

## Advanced Settings

**Maximum number of retries**: The maximum number of retries before the output
returns an error. The retries use an exponential backoff
policy.

**Endpoint**: The SNS service endpoint. If empty, defaults to AWS'
Region-specific endpoint. Otherwise, it must point to an SNS-compatible
endpoint.

**Signature version**: Signature version to use for signing SNS requests.
Defaults to `v4`.

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot
be verified against a valid Certificate Authority (for example, self-signed
certificates). Defaults to toggled on.

## SMS Notifications {#sms}

You can use an Amazon SNS Notification target to send text
messages (SMS) to a list of phone numbers. To do this, you'll need to set up a
allowlist of phone numbers that are permitted to receive Notifications.

1. Select **Notifications** in the sidebar, and then select **Targets** > **Add Target**.
1. Enter a unique **Target ID**.
1. Set the **Target type** to **AWS SNS**.
1. Set **Destination type** to **Phone number**.
1. Set **Region** to the Region of the Amazon S3 bucket.
1. In **Default Phone number**, enter a comma-separated list of phone numbers
   that are allowed to receive Notifications. This value can be overridden by
   the Notification event `__phoneNumber` field. You can use `*` as the wildcard
   character.<br /> For example: `+15555550123, +15555551***`.
1. If desired, use **Phone number allowlist** to specify a wildcard list of allowed phone numbers.
1. Configure the remaining sections of the AWS SNS Notification target as described earlier.
1. Select **Save**.

Now, when you set up [Notifications](notifications), you can select the new
Amazon SNS target and specify any phone number that matches the configured
allowlist.