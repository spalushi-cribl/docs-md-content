# Amazon Kinesis Streams


Cribl Edge can output events to **Amazon Kinesis Data Streams** records of up to 1MB  uncompressed. Cribl Edge does **not** have to run on AWS in order to deliver data to a Kinesis Data Stream. 

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info} 

## Meeting the Needs of Different Downstream Receivers

You can use this Destination to send data to different kinds of downstream receivers that have different expectations. In all cases what the Destination sends out are [Kinesis Records](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_Record.html). This works because the Amazon Kinesis Data Streams Service is flexible about what is actually in a Record, as the two following use cases show.

### Sending Multiple Events within One Record {#multi-events-one-record}

When used to feed data into [Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/), this Destination will start with multiple events as NDJSON, gzip-compress them together into a blob, and send that as a single Kinesis Record. This uses the Kinesis [PutRecord API call](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecord.html) to send the combined events in a single Record.

Firehose will gunzip-uncompress the Record and split the NDJSON out into individual events again. There are also other downstream receivers besides Firehose that can do the same thing.

This approach requires either:

* **Advanced Settings > Compression** set to `Gzip`, **or**
* **Advanced Settings > Send batched** toggled on.

Meanwhile, if you want Cribl Edge to be the consumer, on the Kinesis Streams Source, set **Record data format** to `Cribl`. See the [format details](#format-detail) below.

### Sending One Event Per Record {#one-event-one-record}

In contrast to the first use case, some downstream receivers require a Kinesis Record to contain exactly one Cribl event. Here, this Destination uses the Kinesis [PutRecords API call](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) to send multiple events in a single API call, but where each event has a Record to itself. (Note the difference between `PutRecord` in the previous use case and `PutRecords` in this one.)

This approach requires:
* **Advanced Settings > Compression** set to `None`, **and**
* **Advanced Settings > Send batched** toggled off.

Meanwhile, if you want Cribl Edge to be the consumer, on the Kinesis Streams Source, set **Record data format** to `Newline JSON`. See the [format details](#format-detail) below.

## Configuring Cribl Edge to Output to Amazon Kinesis Data Streams

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Destination** at right. From the resulting drawer's tiles, select **Amazon** > **Kinesis**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.

Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Amazon** > **Kinesis**. Next, click **Add Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Kinesis definition. 

**Stream name**: Enter the name of the Kinesis Data Stream to which to send events.

**Region**: Select the AWS Region where the Kinesis Data Stream is located.

### Optional Settings

**Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.

**Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Edge's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Persistent Queue Settings

> This tab is displayed when the **Backpressure behavior** is set to **Persistent Queue**. 
>
{.box .info}

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the destructive **Clear Persistent Queue** button (described below in this section). A maximum queue size of 1 GB disk space is automatically allocated per PQ‑enabled Destination, per Worker Process. The 1 GB limit is on outbound uncompressed data, and no compression is applied to the queue. 
> 
> This limit is not configurable. If the queue fills up, Cribl Edge will block outbound data. To configure the queue size, compression, queue-full fallback behavior, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Max file size**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Destination will stop queueing data and apply the **Queue‑full** behavior. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, etc. Can be set as high as `1 TB`, unless you've [configured](settings-group-fleet#storage) a different **Max PQ size per Worker Process** in Fleet Settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Edge will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

**Queue-full behavior**: Whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** drops the newest events from being sent out of Cribl Edge, and throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear Persistent Queue**: Click this "panic" button if you want to delete the files that are currently queued for delivery to this Destination. A confirmation modal will appear - because this will free up disk space by permanently deleting the queued data, without delivering it to downstream receivers. (Appears only after **Output ID** has been defined.)

**Strict ordering**: The default `Yes` position enables FIFO (first in, first out) event forwarding. When receivers recover, Cribl Edge will send earlier queued events before forwarding newly arrived events. To instead prioritize new events before draining the queue, toggle this off. Doing so will expose this additional control:
- **Drain rate limit (EPS)**: Optionally, set a throttling rate (in events per second) on writing from the queue to receivers. (The default `0` value disables throttling.) Throttling the queue's drain rate can boost the throughput of new/active connections, by reserving more resources for them. You can further optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).

### Authentication

Use the **Authentication Method** drop-down to select an AWS authentication method.

**Auto**: This default option uses the AWS instance's metadata service to automatically obtain short-lived credentials from the IAM role attached to an EC2 instance, local credentials, sidecar, or other source. The attached IAM role grants Cribl Edge Workers access to authorized AWS resources. Can also use the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. Works only when running on AWS.

For details, see AWS' [Actions, resources, and condition keys for Amazon Kinesis Data Streams](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonkinesisdatastreams.html) documentation.

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, e.g., those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select an API key/secret key pair that you've [configured](/stream/securing-and-monitoring#secrets) in Cribl Edge's secrets manager. A **Create** link is available to store a new, reusable secret.

### Assume Role

**Enable for Kinesis Streams**: Toggle to `Yes` to use Assume Role credentials to access Kinesis Streams.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Advanced Settings

**Endpoint**: Kinesis Stream service endpoint. If empty, the endpoint will be automatically constructed from the region.

> To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
>
{.box .info}

**Signature version**: Signature version to use for signing Kinesis stream requests. Defaults to `v4`.

**Put request concurrency**: Maximum number of ongoing put requests before blocking. Defaults to `5`.

**Max record size (KB, uncompressed)**: Maximum size of each individual record before compression. For non-compressible data, 1MB (the default) is the maximum recommended size.

**Flush period (sec)**: Maximum time between requests. Low settings could cause the payload size to be smaller than its configured maximum.

**Reuse connections**: Whether to reuse connections between requests. The default setting (`Yes`) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (e.g., self-signed certificates). Defaults to `Yes`.

**Compression**: Type of compression to use on records being sent downstream. Defaults to `Gzip`, and when set to `None`, displays the following control:

**Send batched**: Whether to send multiple events batched as a single NDJSON Kinesis record (the default) or send each event as its own Kinesis record, serialized as JSON. When toggled off, displays the following control:

* **Max records per flush**: Maximum number of records to send in a single request to the Amazon Kinesis Data Streams Service [PutRecords](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) API endpoint.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Amazon Kinesis Data Streams Permissions

The following permissions are needed to write to an Amazon Kinesis Stream:

* `kinesis:DescribeStream` - Always required.
* `kinesis:PutRecord` - Required for Gzip compression or batching (when **Advanced Settings > Compression** is set to `Gzip` or **Advanced Settings > Send batched** is toggled on).
* `kinesis:PutRecords` - Required for non-batched mode (when **Advanced Settings > Send batched** is toggled off).

## Record Formats in Detail {#format-detail} 

When sending [multiple events within one record](#multi-events-one-record), the Record format will be as follows:

* A header line provides information about the payload (the one supported type is shown below).
* Each Kinesis record will contain multiple events serialized as Newline-Delimited JSON (NDJSON).
* Record payloads (including header and body) will be gzip-compressed if **Advanced Settings > Compression** is set to `Gzip`. 

```json {title="Sample Kinesis Record"}
{"format":"ndjson","count":8,"size":3960}
{"_raw":"07-03-2018 18:33:51.136 -0700 ERROR TcpOutputFd - Read error. Connection reset by peer","_meta":"timestartpos::0 timeendpos::29 _subsecond::.136 date_second::51 date_hour::18 date_minute::33 date_year::2018 date_month::july date_mday::3 date_wday::tuesday date_zone::-420 punct::--_::._-___-__.____","_time":"1530668031","source":"/mnt-big/ledion/hwf/var/log/foo/food.log","host":"ledion-hwf","sourcetype":"food","index":"_internal","cribl_pipe":"foo2"}
{"_raw":"07-03-2018 18:33:51.136 -0700 INFO  TcpOutputProc - Connection to 127.0.0.1:10000 closed. Read error. Connection reset by peer","_meta":"timestartpos::0 timeendpos::29 _subsecond::.136 date_second::51 date_hour::18 date_minute::33 date_year::2018 date_month::july date_mday::3 date_wday::tuesday date_zone::-420 punct::--_::._-____-___...:_.__.____","_time":"1530668031","source":"/mnt-big/ledion/hwf/var/log/foo/food.log","host":"ledion-hwf","sourcetype":"food","index":"_internal","cribl_pipe":"foo2"}
...
```

When sending [one event per Record](#one-event-one-record), the Record format will be as follows:

* No header will be included.
* Each Kinesis record will contain a single event serialized as JSON (**not** NDJSON).
* Record payloads will not be compressed.
