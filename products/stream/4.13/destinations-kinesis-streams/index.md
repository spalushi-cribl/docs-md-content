# Amazon Kinesis Data Streams Destination


Cribl Stream can output events to **Amazon Kinesis Data Streams** records of up to 1MB  uncompressed. Cribl Stream does **not** have to run on AWS in order to deliver data to a Kinesis Data Stream. 

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info} 

## Meeting the Needs of Different Downstream Receivers

You can use this Destination to send data to different kinds of downstream receivers that have different expectations. In all cases what the Destination sends out are [Kinesis Records](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_Record.html). This works because the Amazon Kinesis Data Streams Service is flexible about what is actually in a Record, as the two following use cases show.

### Sending Multiple Events within One Record {#multi-events-one-record}

When used to feed data into [Amazon Data Firehose](https://aws.amazon.com/kinesis/data-firehose/), this Destination will start with multiple events as NDJSON, gzip-compress them together into a blob, and send that as a single Kinesis Record. This uses the Kinesis [PutRecord API call](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecord.html) to send the combined events in a single Record.

Amazon Data Firehose will gunzip-uncompress the Record and split the NDJSON out into individual events again. There are also other downstream receivers besides Amazon Data Firehose that can do the same thing.

This approach requires either:

* **Advanced Settings > Compression** set to `Gzip`, **or**
* **Advanced Settings > Send batched** toggled on.

Meanwhile, if you want Cribl Stream to be the consumer, on the Kinesis Streams Source, set **Record data format** to `Cribl`. See the [format details](#format-detail) below.

### Sending One Event Per Record {#one-event-one-record}

In contrast to the first use case, some downstream receivers require a Kinesis Record to contain exactly one Cribl event. Here, this Destination uses the Kinesis [PutRecords API call](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) to send multiple events in a single API call, but where each event has a Record to itself. (Note the difference between `PutRecord` in the previous use case and `PutRecords` in this one.)

This approach requires:
* **Advanced Settings > Compression** set to `None`, **and**
* **Advanced Settings > Send batched** toggled off.

Meanwhile, if you want Cribl Stream to be the consumer, on the Kinesis Streams Source, set **Record data format** to `Newline JSON`. See the [format details](#format-detail) below.

## Amazon Kinesis Data Streams Permissions

The following permissions are needed to write to an Amazon Kinesis Stream:

**Shard Verification (Determining Stream Availability)**:
* `kinesis:ListShards` - Required by default to use the **[ListShards API](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_ListShards.html)**. This API offers higher rate limits for shard verification.
* `kinesis:DescribeStream` - Required instead of `kinesis:ListShards` if the **ListShards API** [advanced setting](#listshards) is toggled off.

**Data Writing (Sending Data to the Stream)**:
* `kinesis:PutRecord` - Required for Gzip compression or batching (when **Advanced Settings > Compression** is set to `Gzip` or **Advanced Settings > Send batched** is toggled on).
* `kinesis:PutRecords` - Required for non-batched mode (when **Advanced Settings > Send batched** is toggled off).

## Configure Cribl Stream to Output to Amazon Kinesis Data Streams

In Cribl Stream, configure Amazon CloudWatch Kinesis Data Streams:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Kinesis definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**. 
   - **Description**: Optionally, enter a description.
   - **Stream name**: Enter the name of the Kinesis Data Stream (not ARN) to which to send events.
   - **Region**: Select the AWS Region where your Kinesis Data Stream is located. If your Region isn't listed, you can manually enter it. Additionally, you can specify the service endpoint in the **Endpoint** field under [Advanced Settings](#advanced-tab).
3. Next, you can configure the following **Optional Settings**: 
    - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`. When this field is set to `queue`, see [Persistent Queue Settings](#pq) for details.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Authentication](#auth), [Assume Role](#assume), [Processing](#processing), and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.13/snippets/_destinations-persistent-queue-settings.md]

### Authentication {#auth}

Use the **Authentication Method** drop-down to select an AWS authentication method.

[Snippet not found: content/shared/4.13/snippets/_integrations-aws-auto-auth.md]

For details, see AWS' [Actions, resources, and condition keys for Amazon Kinesis Data Streams](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonkinesisdatastreams.html) documentation.

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, for example, those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select an API key/secret key pair that you've [configured](/stream/securing-secrets) in Cribl Stream's secrets manager. A **Create** link is available to store a new, reusable secret.

### Assume Role {#assume}

When using Assume Role to access resources in a different Region than Cribl Stream, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) (STS) endpoint specific to that Region by using the `CRIBL_AWS_STS_REGION` environment variable on your Worker Node. Setting an invalid Region results in a fallback to the global STS endpoint.

**Enable for Kinesis Streams**: Toggle on to use Assume Role credentials to access Kinesis Streams.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Advanced Settings {#advanced-tab}

**Endpoint**: Kinesis Stream service endpoint. If empty, the endpoint will be automatically constructed from the Region.

> To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
>
{.box .info}

**Signature version**: Signature version to use for signing Kinesis Stream requests. Defaults to `v4`.

**Put request concurrency**: Maximum number of ongoing put requests before blocking. Defaults to `5`.

**Record size limit (KB, uncompressed)**: Maximum size of each individual record before compression. For non-compressible data, 1MB (the default) is the maximum recommended size.

**Flush period (sec)**: Maximum time between requests. Low settings could cause the payload size to be smaller than its configured maximum.

**Compression**: Type of compression to use on records being sent downstream. Defaults to `Gzip`, and when set to `None`, displays the following control:

**Send batched**: Whether to send multiple events batched as a single NDJSON Kinesis record (the default) or send each event as its own Kinesis record, serialized as JSON. When toggled off, displays the following control:

* **Records-per-flush limit**: Maximum number of records to send in a single request to the Amazon Kinesis Data Streams Service [PutRecords](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) API endpoint.

**ListShards API**{{< id listshards >}}: The [ListShards API](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_ListShards.html) is used by default for Kinesis Stream shard verification. This provides significantly higher rate limits (1,000 calls/second/stream), minimizing throttling and improving data delivery speed and reliability, particularly in high-throughput environments.

* Using ListShards requires your IAM policy to have the `kinesis:ListShards` action to grant access. See [Controlling Access to Amazon Kinesis Data Streams Resources Using IAM](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html).

* Toggle off to use the [DescribeStream API](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_DescribeStream.html), which has lower rate limits (10 calls/second/account) and increased susceptibility to throttling.

**Reuse connections**: Whether to reuse connections between requests. Toggling on (default) can improve performance.

**Reject unauthorized certificates**: Whether to accept certificates that cannot be verified against a valid Certificate Authority (for example, self-signed certificates). Defaults to toggled on.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

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

## Troubleshooting

[Snippet not found: content/shared/4.13/snippets/_destinations-troubleshooting.md]