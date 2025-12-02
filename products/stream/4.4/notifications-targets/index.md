# Configuring Targets


To add a new Notification target from the [Manage Notifications](notifications#managing) page's **Targets** tab:

1. Click **Add Target** to open the **New Target** modal shown below.
1. Give this target a unique **Target ID**.
1. Set the **Target type** to either [Webhook](#webhook),
   [PagerDuty](#pagerduty), [Slack](#slack), or [AWS SNS](#amazonsns). Then
   configure the target according to the corresponding section below.

![Add targets on the **Targets** tab](notifications-targets.997cc1fbf2.png)
{border="true"}

> Notifications require an Enterprise or Standard
> [license](licensing#license-types), without which all the target configuration
> options described on this page will be hidden or disabled in Cribl Stream's UI.
>
{.box .info}

## Amazon SNS {#amazonsns}

You can send notifications to an Amazon Simple Notification Service ([SNS](https://aws.amazon.com/sns/)) 
topic. This gives you access to a broad array of notification destinations, such
as various AWS services, mobile push notifications, or [text messages](#sms).

To add an Amazon SNS notification target in Cribl Stream, go to **Manage** > **Notifications** > **Targets** > **Add Target**.

### General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in
the **Target ID** column of the **Targets** tab. It can't be changed later, so 
make sure you like it.

**Target type**: Select **AWS SNS**. 

**Region**: Select the region associated with the Amazon S3 bucket.

**Destination type**: Defaults to **Topic ARN**. The SMS section below explains
the **Phone number** option.

**Default Topic ARN**: The default Amazon Resource Name ([ARN](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html)) of
the Amazon SNS [topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html) 
to which you want to send notifications. Cribl Stream expects the ARN in a format
like this: 

`arn:aws:sns:region:account-id:MyTopic`.

If you use a non-AWS URL, the format must be:

`{url}/myQueueName` – for example, `https://host:port/myQueueName`. 

Must be a JavaScript expression (which can evaluate to a constant value),
enclosed in quotes or backticks. Can be evaluated only at initialization time.
For example, if you're referencing a Global Variable:
`https://host:port/myQueue-${C.vars.myVar}`. This value can be overridden by the
notification event `__topicArn` field.

**Phone number allowlist**: A wildcard list of phone numbers that are allowed to
receive SMS notifications. This is used when **Destination type** is set to
**Phone number**.

### Authentication

**Auto**: This default option uses the AWS instance's metadata service to
automatically obtain short-lived credentials from the IAM role attached to an
EC2 instance, local credentials, sidecar, or other source. The attached IAM role
grants Cribl access to authorized AWS resources. Can also use the environment
variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when
running on AWS.

**Manual**: If not running on AWS, you can select this option to enter a static
set of user-associated IAM credentials (your access key and secret key) directly
or by reference. This is useful for Worker Nodes not in an AWS VPC, like those
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
  that you've configured in Cribl Stream's [secrets manager](securing-and-monitoring#secrets). 
  To store a new, reusable secret, click **Create**.

Now, you can select the new Amazon SNS target when configuring [notifications](notifications).

### Assume Role

**Enable for SNS**: Toggle to **Yes** to define an IAM Role to use, instead of
automatically detecting one locally.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role. This is
required only when assuming a role that requires this ID in order to delegate
third-party access. For details, see [AWS' documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html).

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds.
Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1
hour).

###  Processing Settings  {#processing-settings}

####  Post‑Processing  {#post-processing}

**System fields**: The default field is `cribl_host`, with a value of the Cribl
node that processed the event.

### Advanced Settings

**Maximum number of retries**: The maximum number of retries before the output
returns an error. Errors are retriable. The retries use an exponential backoff
policy.

**Endpoint**: The SNS service endpoint. If empty, defaults to AWS'
Region-specific endpoint. Otherwise, it must point to an SNS-compatible
endpoint.

**Signature version**: Signature version to use for signing SNS requests.
Defaults to `v4`.

**Reuse connections**: Whether to reuse connections between requests. The
default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot
be verified against a valid Certificate Authority (e.g., self-signed
certificates). Defaults to `Yes`.

## PagerDuty Targets  {#pagerduty}

This option sends Cribl Stream Notifications to [PagerDuty](https://developer.pagerduty.com/docs/get-started/getting-started/), a real-time incident response platform, using Cribl Stream's native integration with the PagerDuty API. Select **Target type**: `PagerDuty` to expose the following additional options on the modal's (single) **General Settings** left tab:

**Routing key**: Enter your 32-character Integration key on a PagerDuty service or global ruleset.

**Group**: Optionally, specify a PagerDuty default group to assign to Cribl Stream Notifications.

**Class**: Optionally, specify a PagerDuty default class to assign to Cribl Stream Notifications.

**Component**: Optionally, a PagerDuty default component value to assign to Cribl Stream Notification. (This field is prefilled with `logstream`.)

**Severity**: Set the default message severity for events sent to PagerDuty. Defaults to `info`; you can instead select `error`, `warning`, or `critical`. (Will be overridden by the `__severity` value, if set.)

## Slack {#slack}

You can send notifications to a Slack channel, using Slack's [Incoming Webhooks](https://api.slack.com/messaging/webhooks).

First, in your Slack workspace, use the [Incoming Webhooks
app](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks) to create a URL for the
channel you want to send notifications to.

Then, in Cribl Stream, add a Slack notification target:

1. Go to **Manage** > **Notifications** > **Targets** > **Add Target**.
1. Enter a unique **Target ID**.
1. Set the **Target type** to **Slack**.
1. In **Webhook URL**, add the full URL of your Slack Incoming Webhook.<br />For example: `https://hooks.slack.com/services/T00000000/B00000000/YOUR_DUMMY_SECRET_HERE`.
1. Select **Save**.

Now, you can select the new Slack target when configuring [notifications](notifications).

You can also use a Webhook Destination to send specific event data to Slack for a team's attention. For more information, see our [Slack/Webhook integration topic](/stream/usecase-webhook-slack).

## SMS Notifications {#sms}

You can use an [Amazon SNS](#amazon-sns) notification target to send text
messages (SMS) to a list of phone numbers. To do this, you'll need to set up a
allowlist of phone numbers that are permitted to receive notifications.

1. Go to **Manage** > **Notifications** > **Targets** > **Add Target**.
1. Enter a unique **Target ID**.
1. Set the **Target type** to **AWS SNS**.
1. Set **Region** to the region of the Amazon S3 bucket.
1. Set **Destination type** to **Phone number**.
1. In **Default Phone number**, enter a comma-separated list of phone numbers
   that are allowed to receive notifications. This value can be overridden by
   the notification event `__phoneNumber` field. You can use `*` as the wildcard
   character.<br /> For example: `+15555550123, +15555551***`.
1. **Phone number allowlist** is a wildcard list of allowed phone numbers.
1. See the [Amazon SNS](#amazon-sns) section for details on configuring the **Authentication**, **Processing**, and **Advanced** tabs.
1. Select **Save**.

Now, when you set up [notifications](notifications), you can select the new
Amazon SNS target and specify any phone number that matches the configured
allowlist.

## Webhook Targets  {#webhook}

With this option, you can send Cribl Stream Notifications to an arbitrary webhook. Select **Target type**: `Webhook` to expose multiple left tabs, with the following configuration options:

### General Settings

The added options that appear on this first left tab are:

**URL**: The endpoint that should receive Cribl Stream Notification events.

> To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
>
{.box .info}

**Method**: Select the appropriate HTTP verb for requests: `POST` (the default), `PUT`, or `PATCH`.

**Format**: Specifies how to format Notification events before sending them to the endpoint. Select one of the following:

- `NDJSON` (newline-delimited JSON, the default).
- `JSON Array`.
- `Custom`, which exposes these additional fields:
  - {{< id `source-expression` >}} **Source expression**: JavaScript expression whose evaluation shapes the event that Cribl Stream sends to the endpoint. E.g.: `notification=${_raw}`. For other fields you can use, see [Expression Fields](#fields). If empty, Cribl Stream will send the full notification event as stringified JSON.
  - **Drop when null**: Toggle to `Yes` if you want to drop events where the above **Source expression** evaluates to `null`.
  - **Content type**: Defaults to `application/x‑ndjson`. You can substitute a different content type for requests sent to the endpoint. This entry will be overridden by any content types set in this modal's **Advanced Settings** tab > **Extra HTTP Headers** section.

### Authentication

Select one of the following options for authentication:
- **None**: Don't use authentication.
- **Auth token**: Use HTTP token authentication. In the resulting **Token** field, enter the bearer token that must be included in the HTTP authorization header.
- **Basic**: In the resulting **Username** and **Password** fields, enter HTTP Basic authentication credentials.

### Processing Settings

The options on this left tab are identical to those on the [Webhook Destination](destinations-webhook#processing-settings)'s **Processing Settings** tab, with two exceptions: 
* The default **System fields** entry here is `cribl_host`.
* You cannot specify a post-processing Pipeline here.

### Advanced Settings

The options on this left tab are identical to those on the [Webhook Destination](destinations-webhook#advanced-settings)'s **Advanced Settings** tab.

### Expression Fields {#fields}

When building the [Source expression](#source-expression), you can use the following fields:

#### Fields Common to All Notification Types {#common-fields}

- `starttime`: Beginning of the time bucket where this condition was reported. All Notifications have this.
- `endtime`: End of the time bucket where this condition was reported. All Notifications have this.
- `_time`: Timestamp when this Notification was created. All Notifications have this.
- `cribl_host`: Hostname of the (physical or virtual) machine on which this Notification was created. All Destination and Source Notifications have this.
- `cribl_notification`: Configured name/ID of this Notification. All Destination and Source Notifications have this.
- `origin_metadata`: Object containing metadata about the Notification's origin, with the following fields for all Destination Notifications:
  - `type`: "output".
  - `id`: ID of the affected Destination.
  - `subType`: Destination's type (where applicable).  
- `origin_metadata`: Object containing metadata about the Notification's origin, with the following fields for all Source Notifications:
  - `type`: "input".
  - `id`: ID of the affected Source.
  - `subType`: Source's type (where applicable).

#### Unhealthy Destination

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `output`: Output ID of the affected Destination.
- `_raw`: "Destination `${output}` [in group `${__worker_group}`] is unhealthy".
- `_metric`: "health.outputs".



#### Destination Backpresssure Activated

- `backpressure_type`: `1` for Block, `2` for Drop.
- `output`: Output ID of the affected Destination.
- `_raw`: "Backpressure ([dropping|blocking]) is engaged for destination `${output}` [in group `${__worker_group}`]".
- `_metric`: "backpressure.outputs".

#### Source High Data Volume

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `bytes`: Number of bytes received in the time bucket.
- `input`: Input ID of the affected Source.
- `_raw`: "Source `${input}` [in group `${__worker_group}`] traffic volume greater than
  `${dataVolume}` in `${timeWindow}`".
- `_metric`: "total.in_bytes".

#### Source Low Data Volume

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `bytes`: Number of bytes received in the time bucket.
- `input`: Input ID of the affected Source.
- `_raw`: "Source `${input}` [in group `${__worker_group}`] traffic volume less than
  `${dataVolume}` in `${timeWindow}`".
- `_metric`: "total.in_bytes".

#### Source No Data Received

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `_time`: Timestamp when this Notification was created.
- `input`: Input ID of the affected Source.
- `_raw`: "Source ${input} [in group ${__worker_group}] had no data for ${timeWindow}".
- `_metric`: "total.in_bytes"

#### License Expiration

- `severity`: One of "warn" or "fatal".
- `title`: One of: "License expiring soon, data will stop flowing." Or: "License has expired. Data flow has been stopped.".
- `text`: One of: "License will expire on `${expirationDate}`, no external inputs will be read
  after that time. Please contact sales@cribl.io to renew your license." Or: "License has expired."