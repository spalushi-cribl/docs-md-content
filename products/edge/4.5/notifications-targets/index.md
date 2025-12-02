# Configuring Targets


To add a new Notification target from the [Manage Notifications](notifications#managing) page's **Targets** tab:

1. Click **Add Target** to open the **New Target** modal shown below.
1. Give this target a unique **Target ID**.
1. Set the **Target type** to [Webhook](#webhook), [PagerDuty](#pagerduty), [Slack](#slack), [AWS SNS](#amazonsns), or [Email](#email). Then
   configure the target according to the corresponding section below.

![Add targets on the **Targets** tab](notifications-targets.997cc1fbf2.png)
{border="true"}

When you create a Notification target in Cribl Stream, Edge, or Search, it will also be available to you in the other products. For example, if you create a Notification target in Cribl Stream, you can also access it in Cribl Edge and Cribl Search. 

> Notifications require an Enterprise or Standard
> [license](licensing#license-types), without which all the target configuration
> options described on this page will be hidden or disabled in Cribl Edge's UI.
>
{.box .info}

## Advanced Target Configuration

Click **Delete Target** to delete an existing Notification target.

Click **Clone Target** to copy an existing target. A modal will open so you can modify the configuration of the target you started with.

To edit any target's definition in a JSON text editor, click **Manage as JSON** at the bottom of the **Notifications** > **Targets** modal. You can directly edit multiple values, and you can use the **Import** and **Export** buttons to copy and modify existing Source configurations as `.json` files.

Some targets have additional controls for target configuration. See the documentation for the target of interest for more information.

## Webhook  {#webhook}

With this option, you can send Cribl Edge Notifications to an arbitrary webhook. Select **Webhook** in the **New Target** modal to expose multiple left tabs, with the following configuration options:

### General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in
the **Target ID** column of the **Targets** tab. It can't be changed later, so 
make sure you like it.

### Configuration

The added options that appear on this first left tab are:

**URL**: The endpoint that should receive Cribl Edge Notification events.

> To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
>
{.box .info}

**Method**: Select the appropriate HTTP verb for requests: `POST` (the default), `PUT`, or `PATCH`.

**Format**: Specifies how to format Notification events before sending them to the endpoint. Select one of the following:

- `NDJSON` (newline-delimited JSON, the default).
- `JSON Array`.
- `Custom`, which exposes these additional fields:
  - {{< id `source-expression` >}} **Source expression**: JavaScript expression whose evaluation shapes the event that Cribl Edge sends to the endpoint. E.g.: `notification=${_raw}`. For other fields you can use, see [Expression Fields](#fields). If empty, Cribl Edge will send the full notification event as stringified JSON.
  - **Drop when null**: Toggle to `Yes` if you want to drop events where the above **Source expression** evaluates to `null`.
  - **Content type**: Defaults to `application/x‑ndjson`. You can substitute a different content type for requests sent to the endpoint. This entry will be overridden by any content types set in this modal's **Advanced Settings** tab > **Extra HTTP Headers** section.

### Authentication

Select one of the following options for authentication:
- **None**: Don't use authentication.
- **Auth token**: Use HTTP token authentication. In the resulting **Token** field, enter the bearer token that must be included in the HTTP authorization header.
- **Basic**: In the resulting **Username** and **Password** fields, enter HTTP Basic authentication credentials.

### Post-Processing Settings

The Webhook and AWS SNS targets share most post-processing settings. See [Expression Fields](#fields) for complete documentation.

### Retries {#webhook-retries}

**Honor Retry-After header**: Whether to honor a [`Retry-After` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After), provided that the header specifies a delay no longer than 180 seconds. Cribl Edge limits the delay to 180 seconds even if the `Retry-After` header specifies a longer delay. When enabled, any `Retry-After` header received takes precedence over all other options configured in the **Retries** section. When disabled, all `Retry-After` headers are ignored.

**Settings for failed HTTP requests**: When you want to automatically retry requests that receive particular HTTP response status codes, use these settings to list those response codes. 

For any HTTP response status codes that are not explicitly configured for retries, Cribl Edge applies the following rules:

| Status Code | Action |
|---|---|
Greater than or equal to `400` and less than or equal to `500`. | Drop the request.
Greater than `500`. | Retry the request.

Upon receiving a response code that's on the list, Cribl Edge first waits for a set time interval called the **Pre-backoff interval** and then begins retrying the request. Time between retries increases based on an [exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff) whose base is the **Backoff multiplier**, until the backoff multiplier reaches the **Backoff limit (ms)**. At that point, Cribl Edge continues retrying the request without increasing the time between retries any further.

By default, this Destination has no response codes configured for automatic retries. For each response code you want to add to the list, click **Add Setting** and configure the following settings: 

- **HTTP status code**: A response code that indicates a failed request, for example `429 (Too Many Requests)` or `503 (Service Unavailable)`.
- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

**Retry timed-out HTTP requests**: When you want to automatically retry requests that have timed out, toggle this control on to display the following settings for configuring retry behavior:

- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

### Advanced Settings

**Validate server certs**: Reject certificates not authorized by a CA in the **CA certificate path**, nor by another trusted CA (for example, the system's CA). Defaults to `Yes`.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Compress**: Compresses the payload body before sending. Defaults to `Yes` (recommended).

**Keep alive**: By default, Cribl Edge sends `Keep-Alive` headers to the remote server and preserves the connection from the client side up to a maximum of 120 seconds. Toggle this off if you want Cribl Edge to close the connection immediately after sending a request.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Edge hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Max body size (KB)**: Maximum size of the request body before compression. Defaults to `4096` KB, but you can set this limit to as high as 500 MB (512000 KB).

High values can cause high memory usage per Edge Node, especially if a dynamically constructed URL causes this target to send events to more than one URL. The actual request body size might exceed the specified value because the target adds bytes when it writes to the downstream receiver. Therefore, we recommend that you experiment with the **Max body size** value until downstream receivers reliably accept all events.

**Max events per request**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values can cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Extra HTTP headers**: Name-value pairs to pass to all events as additional HTTP headers. Values will be sent encrypted. You can also add headers dynamically on a per-event basis in the `__headers` field; headers added by this method take precedence over headers defined in the **Extra HTTP headers** table.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Edge will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Used to declare headers that are safe to log as plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

## PagerDuty  {#pagerduty}

This option sends Cribl Edge Notifications to [PagerDuty](https://developer.pagerduty.com/docs/get-started/getting-started/), a real-time incident response platform, using Cribl Edge's native integration with the PagerDuty API. Select Select **PagerDuty** in the **New Target** modal to expose the following additional options on the modal's (single) **General Settings** left tab:

### General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in
the **Target ID** column of the **Targets** tab. It can't be changed later, so 
make sure you like it.

### Configuration

**Routing key**: Enter your 32-character Integration key on a PagerDuty service or global ruleset.

**Group**: Optionally, specify a PagerDuty default group to assign to Cribl Edge Notifications.

**Class**: Optionally, specify a PagerDuty default class to assign to Cribl Edge Notifications.

**Component**: Optionally, a PagerDuty default component value to assign to Cribl Edge Notification. (This field is prefilled with `logstream`.)

**Severity**: Set the default message severity for events sent to PagerDuty. Defaults to `info`; you can instead select `error`, `warning`, or `critical`. (Will be overridden by the `__severity` value, if set.)

### Retries {#pagerduty-retries}

**Honor Retry-After header**: Whether to honor a [`Retry-After` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After), provided that the header specifies a delay no longer than 180 seconds. Cribl Edge limits the delay to 180 seconds even if the `Retry-After` header specifies a longer delay. When enabled, any `Retry-After` header received takes precedence over all other options configured in the **Retries** section. When disabled, all `Retry-After` headers are ignored.

**Settings for failed HTTP requests**: When you want to automatically retry requests that receive particular HTTP response status codes, use these settings to list those response codes. 

For any HTTP response status codes that are not explicitly configured for retries, Cribl Edge applies the following rules:

| Status Code | Action |
|---|---|
Greater than or equal to `400` and less than or equal to `500`. | Drop the request.
Greater than `500`. | Retry the request.

Upon receiving a response code that's on the list, Cribl Edge first waits for a set time interval called the **Pre-backoff interval** and then begins retrying the request. Time between retries increases based on an [exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff) whose base is the **Backoff multiplier**, until the backoff multiplier reaches the **Backoff limit (ms)**. At that point, Cribl Edge continues retrying the request without increasing the time between retries any further.

By default, this Destination has no response codes configured for automatic retries. For each response code you want to add to the list, click **Add Setting** and configure the following settings: 

- **HTTP status code**: A response code that indicates a failed request, for example `429 (Too Many Requests)` or `503 (Service Unavailable)`.
- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

**Retry timed-out HTTP requests**: When you want to automatically retry requests that have timed out, toggle this control on to display the following settings for configuring retry behavior:

- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

## Slack {#slack}

You can send notifications to a Slack channel, using Slack's [Incoming Webhooks](https://api.slack.com/messaging/webhooks).

> You can also use a Webhook Destination to send specific event data to Slack for a team's attention.
> For more information, see our [Slack/Webhook integration topic](/stream/usecase-webhook-slack).
{.box .success}

First, in your Slack workspace, use the [Incoming Webhooks
app](https://slack.com/apps/A0F7XDUAZ-incoming-webhooks) to create a URL for the
channel you want to send notifications to.

Then, in the **New Target** modal, click **Slack**. to expose the following additional options on the modal's (single) **General Settings** left tab:

### General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in
the **Target ID** column of the **Targets** tab. It can't be changed later, so 
make sure you like it.

### Configuration {#slack-target-config}

**Webhook URL**: Add the full URL of your Slack Incoming Webhook. For example: `https://hooks.slack.com/...`.

### Retries {#slack-retries}

**Honor Retry-After header**: When toggled to `Yes` and the `retry-after` [header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) is present, Cribl Edge honors any `retry-after` header that specifies a delay, up to a maximum of 20 seconds. Cribl Edge always ignores `retry-after` headers that specify a delay longer than 20 seconds.

Cribl Edge will log a warning message with the delay value retrieved from the `retry-after` header (converted to ms).

When toggled to `No` (the default), Cribl Edge ignores all `retry-after` headers.

**Settings for failed HTTP requests**: Automatically retries after unsuccessful response status codes, such as `429` (Too Many Requests) or `503` (Service Unavailable). Clicking **Add Setting** reveals a table where you can add retry parameters for individual failed HTTP requests. These include:

- **HTTP status code**: The individual status code for which the retry parameters apply.
- **Pre-backoff interval (ms)**: How long, in milliseconds, Cribl Edge should wait before initiating backoff. The maximum interval is 600,000 ms (10 minutes).
- **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so forth.
- **Backoff limit (ms)**: The maximum backoff interval, in milliseconds, Cribl Edge should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

**Retry timed-out HTTP requests**: When set to `Yes`, Cribl Edge to automatically retries HTTP requests that have timed out. Defaults to `No`. Enabling this option exposes the following settings:

- **Pre-backoff interval (ms)**: How long, in milliseconds, Cribl Edge should wait before initiating backoff. Maximum interval is `600,000 ms` (10 minutes).
- **Backoff multiplier**: Base for exponential backoff. A value of `2` (default) means Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so forth.
- **Backoff limit (ms)**: The maximum backoff interval, in milliseconds, Cribl Edge should apply. Default (and minimum) is 10,000 ms (10 seconds); maximum is 180,000 ms (180 seconds).

## AWS SNS {#amazonsns}

You can send notifications to an Amazon Simple Notification Service ([SNS](https://aws.amazon.com/sns/)) 
topic. This gives you access to a broad array of notification destinations, such
as various AWS services, mobile push notifications, or [text messages](#sms).

To add an Amazon SNS Notification target in Cribl Edge, go to **Manage** > **Notifications** > **Targets** > **Add Target**.

### General Settings

**Target ID**: Enter a unique ID used to identify the target. This will show in
the **Target ID** column of the **Targets** tab. It can't be changed later, so 
make sure you like it.

### Configuration

**Destination type**: Defaults to **Topic ARN**. The SMS section below explains
the **Phone number** option.

**Region**: Select the region associated with the Amazon S3 bucket.

**Default Topic ARN**: The default Amazon Resource Name ([ARN](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html)) of
the Amazon SNS [topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html) 
to which you want to send notifications. Cribl Edge expects the ARN in a format
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
  that you've configured in Cribl Edge's [secrets manager](securing-and-monitoring#secrets). 
  To store a new, reusable secret, click **Create**.

### Assume Role

**Enable for SNS**: Toggle to `Yes` to define an IAM Role to use, instead of
automatically detecting one locally.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role. This is
required only when assuming a role that requires this ID in order to delegate
third-party access. For details, see [AWS' documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html).

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds.
Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1
hour).

####  Post‑Processing  {#post-processing}

The Webhook and AWS SNS targets share most post-processing settings. See [Expression Fields](#fields) for complete documentation.

For AWS SNS, however, there are two exceptions:

- **System fields**: The default field is `cribl_host`, with a value of the Cribl
node that processed the event.

- You cannot specify a post-processing Pipeline here.

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

### SMS Notifications {#sms}

You can use an [Amazon SNS](#amazonsns) Notification target to send text
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
1. See the [Amazon SNS](#amazonsns) section for details on configuring the **Authentication**, **Processing**, and **Advanced** tabs.
1. Select **Save**.

Now, when you set up [notifications](notifications), you can select the new
Amazon SNS target and specify any phone number that matches the configured
allowlist.

## Email {#email}

You can send notifications by email, using an SMTP server of your choice. Once you have configured the email notification target, you can specify the recipients and customize the subject line and message content. See [Email Notifications ](/notifications#email-notifications) for details on configuring an email Notification.

To add an email Notification target in Cribl Edge, go to **Manage** > **Notifications** > **Targets** > **Add Target**.

### General Settings

**Target ID**: Enter a unique name to identify this email Notification target.

### Configuration

**Address**: Identify the SMTP server by its hostname or IP address.

**Port**: Set the SMTP port. Use port `587` for SMTP Secure (SMTPS). You can also use port `25`. Use `465` when SSL/TLS is enabled. You can also use port `2525` if your email service provider supports this port as a backup when other ports are blocked by a network provider or a firewall.

**Encryption type**: Specify the encryption type used to secure SMTP communication. Options include:

- **STARTTLS**: Select this option to start the connection as plaintext, then upgrade it to a TLS-encrypted one if the server supports it. If the server doesn't support it, the connection remains plaintext. 
  > **STARTTLS** upgrades the connection to be encrypted but does not authenticate the server. Take additional steps to prevent man-in-the-middle attacks.
  >
  {.box .warning}
- **Require STARTTLS**: Select this option to require TLS. If the server doesn't support a secure connection, the connection will be dropped.
- **TLS (SMTPS)**: Select this option to use an encrypted connection from the start without requiring a subsequent connection upgrade.
- **None**: Select this option to use a plaintext connection.

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Validate server certs**: Toggle to `Yes` to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (e.g., the system's CA).

**From**: Identify the email address of the sender.

### Authentication

**Username**: The authentication principal (if required).

**Password**: The authentication credential (if required).

### Testing Email Notification Targets

When you finish configuring your email Notification target, you can manage it, using the following options.

Click **Save & Test Target** to save your target and open a modal where you can enter an email to test your email Notification target. After configuration, the button text changes to **Test Target**.

## Expression Fields {#fields}

When building the [Source expression in a Notification target](/notifications-targets#source-expression), you can use the following fields:

### Fields Common to All Notification Types {#common-fields}

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

### Unhealthy Destination

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `output`: Output ID of the affected Destination.
- `_raw`: "Destination `${output}` [in group `${__worker_group}`] is unhealthy".
- `_metric`: "health.outputs".



### Destination Backpressure Activated

- `backpressure_type`: `1` for Block, `2` for Drop.
- `output`: Output ID of the affected Destination.
- `_raw`: "Backpressure ([dropping|blocking]) is engaged for destination `${output}` [in group `${__worker_group}`]".
- `_metric`: "backpressure.outputs".

### Source High Data Volume

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `bytes`: Number of bytes received in the time bucket.
- `input`: Input ID of the affected Source.
- `_raw`: "Source `${input}` [in group `${__worker_group}`] traffic volume greater than
  `${dataVolume}` in `${timeWindow}`".
- `_metric`: "total.in_bytes".

### Source Low Data Volume

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `bytes`: Number of bytes received in the time bucket.
- `input`: Input ID of the affected Source.
- `_raw`: "Source `${input}` [in group `${__worker_group}`] traffic volume less than
  `${dataVolume}` in `${timeWindow}`".
- `_metric`: "total.in_bytes".

### Source No Data Received

- `health`: Numeric value where `0`=green, `1`=yellow, `2`=red.
- `_time`: Timestamp when this Notification was created.
- `input`: Input ID of the affected Source.
- `_raw`: "Source ${input} [in group ${__worker_group}] had no data for ${timeWindow}".
- `_metric`: "total.in_bytes"

### License Expiration

- `severity`: One of "warn" or "fatal".
- `title`: One of: "License expiring soon, data will stop flowing." Or: "License has expired. Data flow has been stopped.".
- `text`: One of: "License will expire on `${expirationDate}`, no external inputs will be read
  after that time. Please contact sales@cribl.io to renew your license." Or: "License has expired."