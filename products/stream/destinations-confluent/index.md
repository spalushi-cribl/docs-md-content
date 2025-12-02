# Confluent Cloud Destination


Cribl Stream supports sending data to [Kafka](https://kafka.apache.org/documentation/) topics on the [Confluent Cloud](https://docs.confluent.io/cloud/current/overview.html) managed Kafka platform.

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes 
> 
> Confluent Cloud uses a binary protocol over TCP. It does not support HTTP proxies, so Cribl Stream must send events directly to receivers. You might need to adjust your firewall rules to allow this traffic.
>
{.box .info} 

## Send Kafka Topic Data to Confluent Cloud

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Destination definition.
   - **Description**: Optionally, enter a description.
   - **Bootstrap servers**: List of Confluent Cloud bootstrap servers to connect to (for example, `myAccount.confluent.cloud:9092`).
   - **Topic**: The topic on which to publish events. Can be overwritten using event's `__topicOut` field.
3. Next, you can configure the following **Optional Settings**: 
     - **Acknowledgments**:  Select the number of required acknowledgments. Defaults to `Leader`.
    - **Record data format**: Format to use to serialize events before writing to Kafka. Defaults to `JSON`. When set to `Protobuf`, the [**Protobuf Format Settings**](#protobuf-settings) section (left tab) becomes available.
    - **Compression**: Codec to use to compress the data before sending to Kafka. Select `None`, `Gzip`, `Snappy`, `LZ4`, or `ZSTD`. Defaults to `Gzip`.
    - **Backpressure behavior**: Select whether to block, drop, or queue incoming events when all receivers are exerting backpressure. Defaults to `Block`. 
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, configure any [Persistent Queue](#pq), [Authentication](#auth), [TLS](#tls_client), [Schema Registry](#registry), [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-tab) settings outlined in the sections below.
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



#### Calculating the Time PQ Will Take to Engage

PQ will not engage until Cribl Stream has exhausted all attempts to send events to the Kafka receiver. This can take several minutes if requests continue to fail or time out. 

To calculate the longest possible time this can take, multiply the values of **Request timeout** and **Retry limit**. For the default values (`60` seconds and `5`, respectively), this would be `60` seconds times `5` retries = 300 seconds, or 5 minutes.

### TLS Settings (Client Side) {#tls_client}

**Enabled** When toggled on (default):

**Autofill?**: This setting is experimental.

**Validate server certs**: Toggle on to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (such as the system's CA).

**Server name (SNI)**: Leave this field blank. See [Connecting to Kafka](#connecting) below.



**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Certificate**: Select an existing certificate name from the drop-down or add a new certificate by selecting **Create**. See [Add a TLS Certificate to a Worker Group](securing-sources-dest#add-certificate-to-group) for details.
 
**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.
 
**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.
 
**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 
 
**Passphrase**: Passphrase to use to decrypt private key.

### Authentication {#auth}

This section documents the SASL (Simple Authentication and Security Layer) authentication settings to use when connecting to bootstrap servers. Using [TLS](#tls_client) is highly recommended.

**Enabled**: Default is toggled off. When toggled on:

* **SASL mechanism**:  Select the SASL (Simple Authentication and Security Layer) authentication mechanism to use. Defaults to `PLAIN`. `SCRAM‑SHA‑256`, `SCRAM‑SHA‑512`, and `GSSAPI/Kerberos` are also available. The mechanism you select determines the controls displayed below.

#### PLAIN, SCRAM-256, or SCRAM-512 

With any of these SASL mechanisms, select one of the following buttons:

**Manual**: Displays **API key** and **Secret** fields to enter your Kafka credentials directly.

**Secret**: This option exposes a **Credentials secret** drop-down in which you can select a [stored text secret](/stream/securing-secrets) that references your Kafka credentials. A **Create** link is available to store a new, reusable secret.

#### GSSAPI/Kerberos 

> Kerberos authentication is not supported for Cribl-managed Workers in Cribl.Cloud, but it is enabled for on-prem Worker Groups, whether the Leader is based in Cribl.Cloud or on-prem.
>
{.box .cloud}

Selecting Kerberos as the authentication mechanism displays the following options:

**Keytab location**: Enter the location of the keytab file for the authentication principal.

**Principal**: Enter the authentication principal, for example: `kafka_user@example.com`.

**Broker service class**: Enter the Kerberos service class for Kafka brokers, for example: `kafka`.

> You will also need to set up your environment and configure the Cribl Stream host for use with Kerberos. See [Kafka Authentication with Kerberos](/stream/usecase-kafka-kerberos) for further detail.
>
{.box .success}

### Schema Registry {#registry}

This section governs Kafka Schema Registry Authentication for data encoded with Avro or JSON schemas, with schemas stored in the Confluent Schema Registry. Cribl Stream determines the schema type by inspecting the schema registered in the Confluent Schema Registry for the provided key and value schema IDs.

**Enabled**: Default is toggled off. When toggled on, displays the following controls:

**Schema registry URL**:  URL for access to the Confluent Schema Registry. (For example, `http://localhost:8081`.)

**Default key schema ID**: Used when `__keySchemaIdOut` is not present to transform key values. Leave blank if key transformation is not required by default. 

**Default value schema ID**: Used when `__valueSchemaIdOut` not present to transform `_raw`. Leave blank if value transformation is not required by default.

**Connection timeout (ms)**: Specifies the maximum time allowed for establishing a successful connection to the schema registry. Defaults to 30000 ms (30 seconds). Valid range is 1000 ms to 60000 ms ( or 1 sec to 60 sec).

**Request timeout (ms)**: Maximum time to wait for a successful request from the schema registry. Defaults to 60000 ms (or 1 minute).

**Retry Limit**: Max number of times to retry requests to the schema registry before dropping the outgoing message. Defaults to 1; enter 0 to not retry at all.

**Authentication enabled**: Toggle on if your schema registry requires authentication.
* **Credentials secret**: Select or **Create** a [stored text secret](/stream/securing-secrets) containing the username and password provided by your schema registry administrator.

**TLS enabled**: defaults to toggled off. When toggled on, displays the following TLS settings for the Schema Registry (in the same format as the [TLS Settings (Client Side)](#tls_client) above):

* **Validate server certs**: Require client to reject any connection that is not authorized by a CA in the **CA certificate path**, or by another trusted CA (such as the system's CA). Default is toggled off.

* **Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

  > With a dedicated Confluent Cloud cluster hosted in Microsoft Azure, be sure to specify the **Server name (SNI)**. If this is omitted, Confluent Cloud might reset the connection to Cribl Stream.
  {.box .warning}

* **Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

* **Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

* **Certificate**: Select an existing certificate name from the drop-down or add a new certificate by selecting **Create**. See [Add a TLS Certificate to a Worker Group](securing-sources-dest#add-certificate-to-group) for details.

* **CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.

* **Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.

* **Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 

* **Passphrase**: Passphrase to use to decrypt private key.

### Processing Settings {#processing}

#### Post‑Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data before it is sent through this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Retries {#retries}

These settings provide flexibility in handling retries for failed messages, allowing you to balance between quick retries and avoiding excessive load on the system.

The default configuration starts with a 300 milliseconds retry interval, doubles the interval after each retry, and caps the maximum retry interval at 30 seconds. The system will attempt to retry the request up to five times before considering it a failure.

**Retry limit**: Maximum number of times to retry a failed request before the message fails. Defaults to `5`. Enter `0` to not retry at all. For example, if set to `5`, the system will attempt to retry the request up to five times before considering it a failure.

**Initial retry interval (ms)**: Initial value used to calculate the retry interval, in milliseconds. This value determines the starting point for the retry delay. The default (and minimum) is `300` ms (three seconds). The maximum allowed value is `600000` ms (10 minutes). For example, if set to `1000` ms, the first retry will occur after one second.

**Backoff multiplier**: Set the backoff multiplier (`2-20`) to control the retry frequency for failed messages. The multiplier is used in an exponential backoff formula to increase the delay between retries. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. For example, with an initial retry interval of `1000` ms and a multiplier of `2`, the retry intervals will be 1,000 ms, 2,000 ms, 4,000 ms, and so on. See the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.

**Backoff limit (ms)**: The maximum wait time for a retry, in milliseconds. This setting caps the exponential backoff delay to prevent excessively long wait times. The default (and minimum) value is `30000` ms (30 seconds), and the maximum is `180000` ms (180 seconds). For example, if the calculated retry interval exceeds 180,000 ms, the retry will occur after 180,000 ms instead.

### Advanced Settings {#advanced-tab}

**Record size limit (KB, uncompressed)**: Maximum size (KB) of each record batch before compression. Setting should be < `message.max.bytes settings` in Kafka brokers. Defaults to `768`.

**Events-per-batch limit**: Maximum number of events in a batch before forcing a flush. Defaults to `1000`.

**Flush period (sec)**: Maximum time between requests. Low values could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Connection timeout (ms)**: Maximum time to wait for a successful connection. Defaults to `10000` ms (10 seconds). Valid range is `1000` to `3600000` ms (1 second to 1 hour).

**Request timeout (ms)**: Maximum time to wait for a successful request. Defaults to `60000` ms (1 minute).

**Authentication timeout (ms)**: Maximum time to wait for Kafka to respond to an authentication request. Defaults to `1000` (1 second).

**Reauthentication threshold (ms)**: If the broker requires periodic reauthentication, this setting defines how long before the reauthentication timeout Cribl Stream initiates the reauthentication. Defaults to `10000` (10 seconds).

A small value for this setting, combined with high network latency, might prevent the Destination from reauthenticating before the Kafka broker closes the connection.

A large value might cause the Destination to send reauthentication messages too soon, wasting bandwidth.

The Kafka setting `connections.max.reauth.ms` controls the reuthentication threshold on the Kafka side.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Protobuf Format Settings {#protobuf-settings}

**Definition set**: From the drop-down, select `OpenTelemetry`. This makes the **Object type** setting available.   

**Object type**: From the drop-down, select `Logs`, `Metrics`, or `Traces`.

#### Working with Protobufs

This Destination supports supports Binary Protobuf payload encoding. The Protobufs it sends can encode traces, metrics, or logs, the three types of telemetry data defined in the OpenTelemetry Project's [Signals](https://opentelemetry.io/docs/concepts/signals/) documentation:

- A **trace** tracks the progression of a single request. 
    - Each trace is a tree of **spans**.
    - A span object represents the work being done by the individual services, or components, involved in a request as that request flows through a system.
- A **metric** provides aggreggated statistical information. 
    - A metric contains individual measurements called **data points**.
- A **log**, in OpenTelemetry terms, is "any data that is not part of a distributed trace or a metric".

When configuring Pipelines (including pre-processing and post-processing Pipelines), you need to ensure that events sent to this Destination conform to the relevant Protobuf specification:

- For traces, [opentelemetry/proto/trace/v1/trace.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v0.9.0/opentelemetry/proto/trace/v1/trace.proto).
- For metrics, [opentelemetry/proto/metrics/v1/metrics.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v0.9.0/opentelemetry/proto/metrics/v1/metrics.proto).
- For logs, [opentelemetry/proto/logs/v1/logs.proto](https://github.com/open-telemetry/opentelemetry-proto/tree/v0.9.0/opentelemetry/proto/logs/v1).

This Destination will drop non-conforming events. If the Destination encounters a parsing error when trying to convert an event to OTLP, it discards the event and Cribl Stream logs the error.

## Connecting to Kafka {#connecting} 

> ##### Leave the **TLS Settings** > **Server name (SNI)** field blank
>
> In Cribl Stream's Kafka-based Sources and Destinations (including this one), the Kafka library that Cribl Stream uses manages SNI (Server Name Indication) without any input from Cribl Stream. Therefore, you should leave the **TLS Settings** > **Server name (SNI)** field blank.
> 
> Setting this field in the Cribl Stream UI can cause traffic to be routed to the wrong brokers, because it interferes with the Kafka library's operation. This is especially important for [Confluent Cloud Dedicated clusters](https://docs.confluent.io/cloud/current/clusters/cluster-types.html), which rely on SNI – as managed by the Kafka library – for routing.
>
{.box .warning}

Connecting to a Kafka cluster involves using hostnames for two key components: bootstrap servers and brokers.

Brokers are servers that comprise the storage layer in a Kafka cluster. Bootstrap servers handle the initial connection to the Kafka cluster, and then return the list of brokers. A broker list can run into the hundreds. Every Kafka cluster has a `bootstrap.servers` [property](https://www.ibm.com/docs/en/iis/11.7?topic=kafka-setting-up-server), defined as either a single `hostname:port` K-V pair, or a list of them. If Cribl Stream tries to connect via one bootstrap server and that fails, Cribl Stream then tries another one on the list.

In the **General Settings** > **Bootstrap servers** list, you can enter either the hostnames of brokers that your Kafka server has been configured to use, or, the hostnames of one or more bootstrap servers. If Kafka returns a list of brokers that's longer than the list you entered, Cribl Stream keeps the full list internally. Cribl Stream neither saves the list nor makes it available in the UI. The connection process simply starts at the beginning whenever the Source or Destination is started.

Here's an overview of the connection process:

1. From the **General Settings** > **Bootstrap servers** list – where each broker is listed as a hostname and port – Cribl Stream takes a hostname and resolves it to an IP address.

2. Cribl Stream makes a connection to that IP address. Notwithstanding the fact that Cribl Stream resolved **one** particular hostname to that IP address, there may be **many** services running at that IP address – each with its own distinct hostname.

3. Cribl Stream establishes TLS security for the connection.

Although SNI is managed by the Kafka library rather than in the Cribl Stream UI, you might want to know how it fits into the connection process. The purpose of the SNI is to specify one hostname – that is, service – among many that might be running on a given IP address within a Kafka cluster. Excluding the other services is one way that TLS makes the connection more secure.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in forwarding data to a Destination. 

Fields for this Destination:

  * `__topicOut`
  * `__key`
  * `__headers`
  * `__keySchemaIdOut`
  * `__valueSchemaIdOut`

## Troubleshooting

The Destination's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](managing-destinations#capture-outgoing-data) to see real-time events as they flow through the Destination. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the delivery process, including any errors or warnings that may have occurred.

**Test**: Ensures that the Destination is correctly set up and reachable. Verify that sample events are sent correctly by clicking **Run Test**.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify delivery issues. Analyze the graphs showing events and bytes in/out over time.