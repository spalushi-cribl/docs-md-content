# Amazon Kinesis Streams


Cribl Stream can output events to **Amazon Kinesis Data Streams** records of up to 1MB  uncompressed. Cribl Stream does **not** have to run on AWS in order to deliver data to a Kinesis Data Stream. 

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info} 

## Configuring Cribl Stream to Output to Amazon Kinesis Data Streams

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Destination** at right. From the resulting drawer's tiles, select **Amazon** > **Kinesis**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.

 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Amazon** > **Kinesis**. Next, click **New Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Kinesis definition. 

**Stream name**: Enter the name of the Kinesis Data Stream to which to send events.

**Region**: Select the AWS Region where the Kinesis Data Stream is located.

### Optional Settings

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

**Queue-full behavior**: Whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** drops the newest events from being sent out of Cribl Stream, and throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear persistent queue**: Click this button if you want to flush out files that are currently queued for delivery to this Destination. A confirmation modal will appear. (Appears only after **Output ID** has been defined.)

### Authentication

Use the **Authentication Method** buttons to select an AWS authentication method.

**Auto**: This default option uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance. The attached IAM role grants Cribl Stream Workers access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

For details, see AWS' [Actions, resources, and condition keys for Amazon Kinesis Data Streams](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonkinesisdatastreams.html) documentation.

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, e.g., those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select an API key/secret key pair that you've [configured](/stream/securing-and-monitoring#secrets) in Cribl Stream's secrets manager. A **Create** link is available to store a new, reusable secret.

### Assume Role


**Enable for Kinesis Streams**: Toggle to `Yes` to use Assume Role credentials to access Kinesis Streams.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.

### Advanced Settings

**Endpoint**: Kinesis Stream service endpoint. If empty, the endpoint will be automatically constructed from the region.

**Signature version**: Signature version to use for signing Kinesis stream requests. Defaults to `v4`.

**Put request concurrency**: Maximum number of ongoing put requests before blocking. Defaults to `5`.

**Max record size (KB, uncompressed)**: Maximum size of each individual record before compression. For non-compressible data, 1MB (the default) is the maximum recommended size.

**Flush period (sec)**: Maximum time between requests. Low settings could cause the payload size to be smaller than its configured maximum.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Format 

Currently, outputted events use the following record format:

* Header line containing information about the payload (currently supports one type, as shown below).
* Newline-Delimited JSON (that is, each Kinesis record will contain multiple events, in **ndjson** format).

Record payloads (including header and body) will be gzip-compressed, and then Kinesis will base64-encode them. 

```json {title="Sample Kinesis Record"}
{"format":"ndjson","count":8,"size":3960}
{"_raw":"07-03-2018 18:33:51.136 -0700 ERROR TcpOutputFd - Read error. Connection reset by peer","_meta":"timestartpos::0 timeendpos::29 _subsecond::.136 date_second::51 date_hour::18 date_minute::33 date_year::2018 date_month::july date_mday::3 date_wday::tuesday date_zone::-420 punct::--_::._-___-__.____","_time":"1530668031","source":"/mnt-big/ledion/hwf/var/log/foo/food.log","host":"ledion-hwf","sourcetype":"food","index":"_internal","cribl_pipe":"foo2"}
{"_raw":"07-03-2018 18:33:51.136 -0700 INFO  TcpOutputProc - Connection to 127.0.0.1:10000 closed. Read error. Connection reset by peer","_meta":"timestartpos::0 timeendpos::29 _subsecond::.136 date_second::51 date_hour::18 date_minute::33 date_year::2018 date_month::july date_mday::3 date_wday::tuesday date_zone::-420 punct::--_::._-____-___...:_.__.____","_time":"1530668031","source":"/mnt-big/ledion/hwf/var/log/foo/food.log","host":"ledion-hwf","sourcetype":"food","index":"_internal","cribl_pipe":"foo2"}
...
```
