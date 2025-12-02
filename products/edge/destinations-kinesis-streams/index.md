# Amazon Kinesis Data Streams Destination


Cribl Edge can output events to **Amazon Kinesis Data Streams** records of up to 1MB  uncompressed. Cribl Edge does **not** have to run on AWS in order to deliver data to a Kinesis Data Stream. 

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

Meanwhile, if you want Cribl Edge to be the consumer, on the Kinesis Streams Source, set **Record data format** to `Cribl`. See the [format details](#format-detail) below.

### Sending One Event Per Record {#one-event-one-record}

In contrast to the first use case, some downstream receivers require a Kinesis Record to contain exactly one Cribl event. Here, this Destination uses the Kinesis [PutRecords API call](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecords.html) to send multiple events in a single API call, but where each event has a Record to itself. (Note the difference between `PutRecord` in the previous use case and `PutRecords` in this one.)

This approach requires:
* **Advanced Settings > Compression** set to `None`, **and**
* **Advanced Settings > Send batched** toggled off.

Meanwhile, if you want Cribl Edge to be the consumer, on the Kinesis Streams Source, set **Record data format** to `Newline JSON`. See the [format details](#format-detail) below.

## Amazon Kinesis Data Streams Permissions

The following permissions are needed to write to an Amazon Kinesis Stream:

**Shard Verification (Determining Stream Availability)**:
* `kinesis:ListShards` - Required by default to use the **[ListShards API](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_ListShards.html)**. This API offers higher rate limits for shard verification.
* `kinesis:DescribeStream` - Required instead of `kinesis:ListShards` if the **ListShards API** [advanced setting](#listshards) is toggled off.

**Data Writing (Sending Data to the Stream)**:
* `kinesis:PutRecord` - Required for Gzip compression or batching (when **Advanced Settings > Compression** is set to `Gzip` or **Advanced Settings > Send batched** is toggled on).
* `kinesis:PutRecords` - Required for non-batched mode (when **Advanced Settings > Send batched** is toggled off).

## Configure Cribl Edge to Output to Amazon Kinesis Data Streams

In Cribl Edge, configure Amazon CloudWatch Kinesis Data Streams:

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Kinesis definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**. 
   - **Description**: Optionally, enter a description.
   - **Stream name**: Enter the name of the Kinesis Data Stream (not ARN) to which to send events.
   - **Region**: Select the AWS Region where your Kinesis Data Stream is located. If your Region isn't listed, you can manually enter it. Additionally, you can specify the service endpoint in the **Endpoint** field under [Advanced Settings](#advanced-tab).
3. Next, you can configure the following **Optional Settings**: 
    - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`. When this field is set to `queue`, see [Persistent Queue Settings](#pq) for details.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Authentication](#auth), [Assume Role](#assume), [Processing](#processing), and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Persistent Queue Settings {#pq}

The **Persistent Queue Settings** tab displays when the **Backpressure behavior** option in **General settings** is set to **Persistent Queue**. Persistent queue buffers and preserves incoming events when a downstream Destination has an outage or experiences backpressure.

Before enabling persistent queue, learn more about persistent queue behavior and how to optimize it with
your system:

- [About Persistent Queues](/persistent-queues)
- [Optimize Destination Persistent Queues (dPQ)](/persistent-queues-destinations)
- [Destination Backpressure Triggers](/destinations-backpressure-triggers)

> On Cribl-managed [Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the destructive **Clear Persistent Queue** button (described at the end of this section). A maximum queue size of 1 GB disk space is automatically allocated per PQ‑enabled Destination, per Worker Process. The 1 GB limit is on outbound uncompressed data, and no compression is applied to the queue.
>
> This limit is not configurable. If the queue fills up, Cribl Stream/Edge will block outbound data. To configure the queue size, compression, queue-full fallback behavior, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Mode:** Use this menu to select when Cribl Stream/Edge engages the persistent queue in response to backpressure events from this Destination. The options are:

   | Mode | Description |
   | ---- | ----------- |
   | **Error** | Queues and stores data on a disk when the Destination is unavailable or in an error state. |
   | **Backpressure** | Queues and stores data to a disk when it detects backpressure from the Destination until the backpressure event resolves. |
   | **Always On** | Cribl Stream or Edge immediately queues and stores all data on a disk for all events, even when there is no backpressure. |

> If a Worker/Edge Node starts with an invalid **Mode** setting, it automatically switches to **Error** mode.
> This might happen if the Worker/Edge Node is running a version that does not support other modes (older than 4.9.0),
> or if it encounters a nonexistent value in YAML configuration files.
{.box .warning}

**File size limit**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Queue size limit**: The maximum amount of disk space that the queue can consume on each Worker Process. When the queue reaches this limit, the Destination stops queueing data and applies the **Queue‑full** behavior. Defaults to `5` GB. This field accepts positive numbers with units of `KB`, `MB`, `GB`, and so on. You can set it as high as `1 TB`, unless you've [configured](group-settings#storage) a different **Worker Process PQ size limit** on the Worker Group/Fleet Settings page.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. Cribl Stream/Edge will append `/<worker‑id>/<output‑id>` to this value.

**Compression**: Set the codec to use when compressing the persisted data after closing a file. Defaults to `None`. `Gzip` is also available.

**Queue-full behavior**: Whether to block or drop events when the queue begins to exert backpressure. A queue begins to exert backpressure when the disk is low or at full capacity. This setting has two options:

  - **Block**: The output will refuse to accept new data until the receiver is ready. The system will return block signals back to the sender.
  - **Drop new data**: Discard all new events until the backpressure event has resolved and the receiver is ready.

**Backpressure duration Limit**: When **Mode** is set to `Backpressure`, this setting controls how long to wait during network slowdowns before activating queues. A shorter duration enhances critical data loss prevention, while a longer duration helps avoid unnecessary queue transitions in environments with frequent, brief network fluctuations. The default value is `30` seconds.

**Strict ordering**: Toggle on (default) to enable FIFO (first in, first out) event forwarding, ensuring Cribl Stream/Edge sends earlier queued events first when receivers recover. The persistent queue flushes every 10 seconds in this mode. Toggle off to prioritize new events over queued events, configure a custom drain rate for the queue, and display this option:

  - **Drain rate limit (EPS)**: Optionally, set a throttling rate (in events per second) on writing from the queue to receivers. (The default `0` value disables throttling.) Throttling the queue drain rate can boost the throughput of new and active connections by reserving more resources for them. You can further optimize Worker startup connections and CPU load in the [Worker Processes](group-settings#processes) settings.

**Clear Persistent Queue**: For Cloud Enterprise only, click this button if you want to delete the files that are currently queued for delivery to this Destination. If you click this button, a confirmation modal appears. Clearing the queue frees up disk space by permanently deleting the queued data, without delivering it to downstream receivers. This button only appears after you define the **Output ID**.

> Use the **Clear Persistent Queue** button with caution to avoid data loss. See [Steps to Safely Disable and Clear Persistent Queues](/persistent-queues-destinations#clear-pq) for more information.
>
{.box .warning}



### Authentication {#auth}

Use the **Authentication Method** drop-down to select an AWS authentication method.

**Auto**: This default option uses the [AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-credentials-node.html) to automatically obtain credentials in the following order of attempts:

1. **IAM Roles for Amazon EC2**: Loaded from AWS Identity and Access Management (IAM) roles attached to an EC2 instance.
1. **Shared Credentials File**: Loaded from the shared credentials file (`~/.aws/credentials`).
1. **Environment Variables**: Loaded from environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
1. **JSON File on Disk**: Loaded from a JSON file on disk.
1. **Other Credential-Provider Classes**: Other credential-provider classes provided by the AWS SDK for JavaScript.

The `Auto` method works both when running on AWS and in other environments where the necessary credentials are available through one of the above methods.

> ##### SSO Providers
> When using the auto authentication method, you can leverage SSO providers like [SAML](usecase-sso-saml#temp-access) and [Okta](usecase-sso-okta#temp-access) to issue temporary credentials. These credentials should be set in the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`. The AWS SDK will then use these environment variables to authenticate.
{.box .info}

For details, see AWS' [Actions, resources, and condition keys for Amazon Kinesis Data Streams](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonkinesisdatastreams.html) documentation.

**Manual**: If not running on AWS, you can select this option to enter a static set of user-associated IAM credentials (your access key and secret key) directly or by reference. This is useful for Workers not in an AWS VPC, for example, those running a private cloud. The **Manual** option exposes these corresponding additional fields:

- **Access key**: Enter your AWS access key. If not present, will fall back to the `env.AWS_ACCESS_KEY_ID` environment variable, or to the metadata endpoint for IAM role credentials.

- **Secret key**: Enter your AWS secret key. If not present, will fall back to the `env.AWS_SECRET_ACCESS_KEY` environment variable, or to the metadata endpoint for IAM credentials.

**Secret**: If not running on AWS, you can select this option to supply a stored secret that references an AWS access key and secret key. The **Secret** option exposes this additional field:

- **Secret key pair**: Use the drop-down to select an API key/secret key pair that you've [configured](/stream/securing-secrets) in Cribl Edge's secrets manager. A **Create** link is available to store a new, reusable secret.

### Assume Role {#assume}

When using **Assume Role** to access resources in a different Region than Cribl Edge, you can target the [AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) (STS) endpoint specific to that Region by using the `CRIBL_AWS_STS_REGION` environment variable on your Edge Node. To specify a custom STS endpoint, set the `CRIBL_AWS_STS_ENDPOINT` environment variable. If both are set, `CRIBL_AWS_STS_ENDPOINT` takes precedence. Setting an invalid Region or endpoint results in a fallback to the global STS endpoint.

**Enable for Kinesis Streams**: Toggle on to use Assume Role credentials to access Kinesis Streams.

**AssumeRole ARN**: Enter the Amazon Resource Name (ARN) of the role to assume.

**External ID**: Enter the External ID to use when assuming role.

**Duration (seconds)**: Duration of the Assumed Role's session, in seconds. Minimum is 900 (15 minutes). Maximum is 43200 (12 hours). Defaults to 3600 (1 hour).

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

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

The Destination's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](managing-destinations#capture-outgoing-data) to see real-time events as they flow through the Destination. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the delivery process, including any errors or warnings that may have occurred.

**Test**: Ensures that the Destination is correctly set up and reachable. Verify that sample events are sent correctly by clicking **Run Test**.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify delivery issues. Analyze the graphs showing events and bytes in/out over time.