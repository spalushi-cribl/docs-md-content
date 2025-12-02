# Splunk HEC


The **Splunk HEC** Destination can stream data to a Splunk [HEC](http://dev.splunk.com/view/event-collector/SP-CAAAE6M) (HTTP Event Collector) receiver through the event, raw, and S2S [endpoints](https://docs.splunk.com/Documentation/Splunk/latest/Data/HECRESTendpoints).

The data arrives to Splunk cooked and parsed, so it enters at the Splunk [data pipeline](https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Datapipeline)'s [indexing](https://docs.splunk.com/Documentation/Splunk/latest/Indexer/Howindexingworks) segment.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
> 
{.box .info}

## Configuring Cribl Stream to Output to Splunk HEC Destinations

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Destination** at right. From the resulting drawer's tiles, select **Splunk** > **HEC**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.

 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Splunk** > **HEC**. Next, click **New Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Splunk HEC definition.

**Load balancing**: Set to `No` by default. When toggled to `Yes`, see [Load Balancing Settings](#lb) below. With the default `No` setting, if you notice that Cribl Stream is not sending data to all possible IP addresses, enable **Advanced Settings** > **Round-robin DNS**.

**Splunk HEC endpoint**: URL of a Splunk HEC endpoint to send events to (e.g., `http://myhost.example.com:8088/services/collector/event`). This setting appears only when **Optional Settings** > **Load balancing** is toggled to `Off`.

> For Splunk Cloud endpoints, change the default `http:` prefix to: `https:`.
>
{.box .success}

### Authentication Settings  {#authentication-settings}

Use the **Authentication method** buttons to select one of these options:

- **Manual**: Displays an **HEC Auth token** field for you to enter your Splunk HEC API access token. 

- **Secret**: This option exposes an **HEC Auth token (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-and-monitoring#secrets) that references the API access token described above. A **Create** link is available to store a new, reusable secret.

### Optional Settings

**Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.   

**Tags**: Optionally, add tags that you can use for filtering and grouping at the final destination. Use a tab or hard return between (arbitrary) tag names.

> For Splunk Cloud endpoints, change the **Splunk HEC endpoint**'s default `http:` prefix to: `https:`
> 
> This Destination does not support Mutual TLS (mTLS).
>
{.box .warning}

### Load Balancing Settings {#lb}

Enabling the **Load balancing** slider displays the following controls:

#### Exclude Current Host IPs

This slider determines whether to exclude all IPs of the current host from the list of any resolved hostnames. Defaults to `No`.

#### Splunk HEC Endpoints {#splunk-hec-endpoints}

The **Splunk HEC Endpoints** table is where you specify a known set of receivers on which to load-balance data. Click **+ Add Endpoint** to specify more receivers on new rows. Each row provides the following fields:

**HEC Endpoint**: Specify the URL to a Splunk HEC [endpoint](https://docs.splunk.com/Documentation/Splunk/latest/Data/HECRESTendpoints) to send events to. See the Splunk [documentation]( https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Cloud) for details on getting your URL. For Splunk Cloud endpoints, `https` protocol is recommended. Supported endpoints include:
  * `/services/collector/event` (default)
  * `/services/collector/raw`
  * `/services/collector/health`
  * `/services/collector/s2s`

**Load weight**: Specify a weight to apply to the receiver for load-balancing purposes.

The final column provides an `X` button to delete any row from the table.

> When you first enable load balancing, or if you edit the load weight once your data is load–balanced, give the logic time to settle. The changes might take a few seconds to register.
>
{.box .info}

See the [explanation of how load balancing works](#lb-how) below.

### Persistent Queue Settings

> This section is displayed when the **Backpressure behavior** is set to **Persistent Queue**.
>
{.box .info}

**Max file size**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Max queue size**: The maximum amount of disk space the queue is allowed to consume. Once this limit is reached, Cribl Stream stops queueing and applies the fallback **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Stream will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

**Queue-full behavior**: Whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear persistent queue**: Click this button if you want to flush out files that are currently queued for delivery to this Destination. A confirmation modal will appear. (Appears only after **Output ID** has been defined.)

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.

### Advanced Settings

**Output multi–metrics**: Toggle to `Yes` to output multiple-measurement metric data points. (Supported in Splunk 8.0 and above, this format enables sending multiple metrics in a single event, improving the efficiency of your Splunk capacity.)

**Validate server certs**: Toggle to `Yes` to reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA).

**Round–robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts. (This setting is available only when **General Settings** > **Load balancing** is set to `No`.)

**Compress**: Toggle to `Yes` to compress the payload body before sending.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Stream hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`. Each request can potentially hit a different HEC receiver.

**Max body size (KB)**: Maximum size, in KB, of the request body. Defaults to `4096`. Lowering the size can potentially result in more parallel requests and also cause outbound requests to be made sooner.

**Max events per request**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values can cause the payload size to be smaller than the configured **Max body size**. Defaults to `1`.

> * Retries happen on this flush interval. 
> * Any HTTP response code in the `2xx` range is considered success. 
> * Any response code in the `5xx` range is considered a retryable error, which will not trigger Persistent Queue (PQ) usage.
> * Any other response code will trigger PQ (if PQ is configured as the Backpressure behavior).
>
{.box .info}

**Extra HTTP headers**: Click **+ Add Header** to add **Name**/**Value** pairs to pass as additional HTTP headers.

> The next two fields take effect only in the [Cribl App for Splunk](/stream/deploy-splunkapp). (Cribl Stream ignores their values.)
>
{.box .info}

**Next processing queue**: Specify the next Splunk processing queue to send the events to, after HEC processing. Defaults to `indexQueue`.

**Default _TCP_ROUTING**: Specify the value of the `_TCP_ROUTING` field for events that do not have `_ctrl._TCP_ROUTING` set. Defaults to `nowhere`. This is useful only when you expect the HEC receiver to route this data on to another destination.

> The next two fields appear only when the **General Settings** > **Load balancing** option is set to `Yes`.
>
{.box .info}

**DNS resolution period (seconds)**: Re-resolve any hostnames after each interval of this many seconds, and pick up destinations from A records. Defaults to `600` seconds.

**Load balance stats period (seconds)**: The duration (in seconds) Cribl Stream tracks historical traffic data to influence load balancing decisions. Defaults to `300` seconds. If a hostname resolves to multiple IPs (via DNS A records), all IPs inherit the weight assigned to the hostname unless each IP is explicitly configured with its own weight. Cribl Stream prioritizes IP-specific settings over hostname-derived settings for load balancing.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Stream will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## How Does Load Balancing Work {#lb-how}

Cribl Stream will attempt to load-balance outbound data as fairly as possibly across all HEC endpoints. For example, if FQDNs/hostnames are used as the Destination addresses, and each resolves to 5 (unique) IPs, then each Worker Process will have its # of outbound connections = {# of IPs x # of FQDNs} for purposes of the Destination. 

Data is sent by all Worker Processes to all endpoints simultaneously, and the amount sent to each receiver depends on these parameters: 

1. Respective destination **weight**.
2. Respective destination **historical data**.

By default, historical data is tracked for 300s. Cribl Stream uses this data to influence the traffic sent to each destination, to ensure that differences decay over time, and that total ratios converge towards configured weights. 

#### Example

Suppose we have two receivers, A and B, each with weight of `1` (i.e., they are configured to receive equal amounts of data). Suppose further that the load-balance stats period is set at the default `300s` and – to make things easy – for each period, there are 200 events of equal size (Bytes) that need to be balanced. 

Interval | Time Range | Events to be dispensed
--- | --- | ---
1 | *time=0s* ---> *time=300s* | **200** 

   Both A and B start this interval with 0 historical stats each.

   Let's assume that, due to various circumstances, 200 events are "balanced" as follows:
   `A = 120 events` and `B = 80 events` – a difference of **40 events** and a ratio of **1.5:1**.

Interval | Time Range | Events to be dispensed
--- | --- | ---
2 | *time=300s* ---> *time=600s* | **200**

At the beginning of interval 2, the load-balancing algorithm will look back to the previous interval stats and carry **half** of the receiving stats forward. I.e., receiver A will start the interval with **60** and receiver B with **40**. To determine how many events A and B will receive during this next interval, Cribl Stream will use their weights and their stats as follows:

Total number of events: `events to be dispensed + stats carried forward = 200 + 60 + 40 = 300`.
Number of events per each destination (weighed): `300/2 = 150` (they're equal, due to equal weight).
Number of events to send to each destination `A: 150 - 60 = 90` and `B: 150 - 40 = 110`.

Totals at end of interval 2: `A=120+90=210`, `B=80+110=190`, a difference of **20 events** and a ratio of **1.1:1**.

Over the subsequent intervals, the difference becomes exponentially less pronounced, and eventually insignificant. Thus, the load gets balanced fairly. 

> If a request fails, Cribl Stream will resend the data to a different endpoint. Cribl Stream will block only if **all** endpoints are experiencing problems.
>
{.box .info}

## Notes on HTTP-based Outputs

- Cribl Stream will attempt to use keepalives to reuse a connection for multiple requests. After two minutes of the first use, the connection will be thrown away, and a new connection will be reattempted. This is to prevent sticking to a particular Destination when there is a constant flow of events. 

- If the server does not support keepalives – or if the server closes a pooled connection while idle – a new connection will be established for the next request.

- When resolving the Destination's hostname with load balancing disabled, Cribl Stream will pick the first IP in the list for use in the next connection. Enable Round-robin DNS to better balance distribution of events between Splunk HEC servers.

* See [Splunk's documentation](https://docs.splunk.com/Documentation/SplunkCloud/latest/Data/Configureindex-timefieldextraction#Add_an_entry_to_fields.conf_for_the_new_field) on editing `fields.conf` to ensure the visibility of index-time fields sent to Splunk by Cribl Stream.
