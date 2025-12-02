# Dynatrace OTLP Destination


The Dynatrace OTLP Destination sends telemetry data, including traces, logs, and metrics, to [Dynatrace](https://docs.dynatrace.com/docs) using the OpenTelemetry Protocol (OTLP). It can send data to various Dynatrace endpoints, including SaaS, Environment ActiveGate, and Containerized Environment ActiveGate.

To get started, connecting to Dynatrace is simple. Select your [endpoint](https://docs.dynatrace.com/docs/extend-dynatrace/opentelemetry/getting-started/otlp-export) and provide your [access token](https://docs.dynatrace.com/docs/dynatrace-api/basics/dynatrace-api-authentication). Need more detailed instructions or configuration options? See below!

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes
>
{.box .info}

## Configure a Dynatrace OTLP Destination {#configuring}

In Cribl Edge, set up a Dynatrace OTLP Destination.

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
    * To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing**.
    * To configure via the [Routes](routes), select **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.

1. In the Destination modal, configure the following under **General Settings**:
    * **Output ID**: Enter a unique name to identify this output definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**. A **Description** is optional.
    * **OTLP version**: Already set to `1.3.1` and cannot be changed.
    * **Protocol**: Already set to `HTTP` and cannot be changed. Uses Binary Protobuf encoding (does not support JSON Protobuf).
    * **Endpoint type**: Select the type of Dynatrace endpoint where you want to send your data.
      * **SaaS**: Sends data to Dynatrace's SaaS endpoint.
      * **ActiveGate**: Choose this option if you are routing data through an ActiveGate.
    * **Endpoint**: The [endpoint URL](https://docs.dynatrace.com/docs/extend-dynatrace/opentelemetry/getting-started/otlp-export) for the Dynatrace environment. You'll need to provide your Dynatrace [environment ID](https://docs.dynatrace.com/docs/get-started/monitoring-environment). This ID uniquely identifies your Dynatrace environment and is required for routing data correctly. You can find your environment ID in your Dynatrace account settings.
      * **SaaS** defaults to `https://{your-environment-id}.live.dynatrace.com/api/v2/otlp`. Replace the `{your-environment-id}` placeholder with your environment ID.
      * **ActiveGate** defaults to `https://{your-activegate-domain}:9999/e/{your-environment-id}/api/v2/otlp`. Replace the `{your-activegate-domain}` placeholder with your ActiveGate domain, and `{your-environment-id}` with your environment ID. Your ActiveGate domain is displayed in your Dynatrace settings in the **Details** section of an ActiveGate under the **Hostname** field. The format should be a fully qualified domain name (FQDN) or IP address.
      * You can specify a custom endpoint using either a valid URL or an IP address (IPv4 or IPv6). Enclose IPv6 addresses in square brackets (for example, `[2001:db8:85a3:0000:0000:8a2e:0370:7334]`).
      * To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).
    * **Auth token (text secret)**: Create or select a Cribl [stored text secret](/stream/securing-secrets) that contains your Dynatrace API [access token](https://docs.dynatrace.com/docs/dynatrace-api/basics/dynatrace-api-authentication), ensuring secure authentication for data transmission. You can generate this token in your Dynatrace account settings under the API tokens section. Ensure that the token has the necessary permissions for the data you are sending.

    * **Optional Settings**:
      * You can [customize the path](#custom-path) instead of using the default cloud endpoints. This is useful when integrating with a hosted Lightstep satellite.
      * **Backpressure behavior**: Whether to `Block`, `Drop`, or [`Persistent Queue`](#pq) events when all receivers are exerting backpressure.
      * **Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Edge's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

    

1. The **Processing Settings**, **Retries**, and **Advanced Settings** provide granular control over how data is handled and transmitted.
   * [Processing Settings](#processing) allow you to define pre-processing and post-processing Pipelines, ensuring that events conform to the relevant Protobuf specifications for traces, metrics, and logs. This includes adding metadata, applying transformations, and conditioning the data.
   * [Retries](#retries) allow you to specify how the system should handle different HTTP response status codes and timeouts, ensuring reliable data delivery.
   * [Advanced Settings](#advanced-settings) offer additional configuration options that enhance the flexibility and performance of data transmission.
    

1. Select **Save**, then **Commit & Deploy**.

1. Use the **Test** tab in the Destination’s configuration modal to validate the configuration and connectivity of your Destination.

#### Custom HTTP Path {#custom-path}

To send data to a custom location, override the default endpoint paths. When the **Protocol** is set to `HTTP`, this Destination sends data to the following endpoints by default:

* Traces: `<endpoint>/v1/traces`
* Metrics: `<endpoint>/v1/metrics`
* Logs: `<endpoint>/v1/logs`

`<endpoint>` refers to the base URL you specified in the **Endpoint** setting above.

To override a default path, provide only the desired path in the corresponding field: **Traces endpoint override**, **Metrics endpoint override**, or **Logs endpoint override**.



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



### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output – both metric events, as dimensions; and log events, as labels. Supports wildcards.

By default, includes `cribl_pipe` (Cribl Edge Pipeline that processed the event).

Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries {#retries}



**Honor Retry-After header**: Toggle on to honor a [`Retry-After` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After), provided that the header specifies a delay no longer than 180 seconds. Cribl Stream/Edge limits the delay to 180 seconds even if the `Retry-After` header specifies a longer delay. Any `Retry-After` header received takes precedence over all other options configured in the **Retries** section. Toggle off to ignore all `Retry-After` headers.

**Settings for failed HTTP requests**: When you want to automatically retry requests that receive particular HTTP response status codes, use these settings to list those response codes. 

For any HTTP response status codes that are not explicitly configured for retries, Cribl Stream/Edge applies the following rules:

| Status Code | Action |
| --- | --- |
| Any in the `1xx`, `3xx`, or `4xx` series | Drop the request |
| Any in the `5xx` series                  | Retry the request |

Upon receiving a response code that's on the list, Cribl Stream/Edge first waits for a set time interval called the **Pre-backoff interval** and then begins retrying the request. Time between retries increases based on an [exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff) whose base is the **Backoff multiplier**, until the backoff multiplier reaches the **Backoff limit (ms)**. At that point, Cribl Stream/Edge continues retrying the request without increasing the time between retries any further.

If the sender (which manages the connection to the Destination) is at capacity, it will not accept any incoming events. These incoming events originate internally from a previous stage of the data flow when Destinations send outbound requests to their respective external services, and they include retry requests and new requests. Any events that were already in transit when the sender reached capacity will continue to be processed downstream.

Sender capacity is freed up when an outgoing request succeeds or encounters a non-retryable error. When the sender has available capacity again, it will resume accepting incoming events. This capacity management is influenced by the number of active connections and configured limits, such as concurrency and buffer sizes. If a Pipeline sends events faster than the Destination can process, the buffers may fill up, leading to backpressure and `Sender at capacity` warnings. This backpressure prevents the sender from accepting additional requests until capacity is restored.

By default, this Destination has no response codes configured for automatic retries. For each response code you want to add to the list, select **Add Setting** and configure the following settings: 

- **HTTP status code**: A response code that indicates a failed request, for example `429 (Too Many Requests)` or `503 (Service Unavailable)`.
- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Stream/Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Stream/Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

**Retry timed-out HTTP requests**: Toggle on to automatically retry requests that have timed out and display the following settings for configuring retry behavior:

- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Stream/Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Stream/Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

### Advanced Settings  {#advanced-settings}


**Validate server certs**: Toggle on to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (e.g., the system's CA).

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Keep alive**: By default, Cribl Edge sends `Keep-Alive` headers to the remote server and preserves the connection from the client side up to a maximum of 120 seconds. Toggle off if you want Cribl Edge to close the connection immediately after sending a request.

**Compression**: Compression type to apply to messages sent to the OpenTelemetry endpoint. `Gzip` (the default) and `None` are available for both protocols. 

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30` sec.

**Request concurrency**: Maximum number of concurrent requests before blocking. This is set per Worker Process. Defaults to `5`.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `2048` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events. See Dynatrace's [best practices](https://docs.dynatrace.com/docs/extend-dynatrace/opentelemetry/best-practices/metrics#limit-payload-size).

**Flush period (sec)**: Maximum time between requests. Low values could cause the payload size to be smaller than the configured **Max body size**. Defaults to `1` sec.

**Extra HTTP headers**: Select **Add Header** to define additional HTTP headers to pass to all events. Each row is a **Name**‑**Value** pair. Values will be sent encrypted.

**Safe headers**: Add headers here to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: Optionally, specify a single Git branch on which to enable this configuration. If this field is empty, the config will be enabled everywhere.



## Protocol and Transport Support {#protocol}

This Destination leverages the OpenTelemetry Protocol (OTLP) [v1.3.1](https://github.com/open-telemetry/opentelemetry-proto/tree/v1.3.1) to send observability data to Dynatrace. OTLP defines Protocol buffer (Protobuf) schemas for its payloads (requests and responses). With the HTTP transport, this Destination supports Binary Protobuf payload encoding, but currently does not support JSON Protobuf.

When configuring Pipelines (including pre-processing and post-processing Pipelines), you need to ensure that events sent to this Destination conform to the relevant Protobuf specification.

- For traces, [opentelemetry/proto/trace/v1/trace.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v1.3.1/opentelemetry/proto/trace/v1/trace.proto)
- For metrics, [opentelemetry/proto/metrics/v1/metrics.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v1.3.1/opentelemetry/proto/metrics/v1/metrics.proto)
- For logs, [opentelemetry/proto/logs/v1/logs.proto](https://github.com/open-telemetry/opentelemetry-proto/blob/v1.3.1/opentelemetry/proto/logs/v1/logs.proto)

This Destination will drop non-conforming events. If the Destination encounters a parsing error when trying to convert an event to OTLP, it discards the event and Cribl Edge logs the error.

## Troubleshoot

The Destination's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](managing-destinations#capture-outgoing-data) to see real-time events as they flow through the Destination. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the delivery process, including any errors or warnings that may have occurred.

**Test**: Ensures that the Destination is correctly set up and reachable. Verify that sample events are sent correctly by clicking **Run Test**.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify delivery issues. Analyze the graphs showing events and bytes in/out over time.