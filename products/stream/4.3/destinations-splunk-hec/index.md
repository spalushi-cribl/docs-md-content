# Splunk HEC


The **Splunk HEC** Destination can stream data to a Splunk [HEC](http://dev.splunk.com/view/event-collector/SP-CAAAE6M) (HTTP Event Collector) receiver through the event, raw, and S2S [endpoints](https://docs.splunk.com/Documentation/Splunk/latest/Data/HECRESTendpoints).

The data arrives to Splunk cooked and parsed, so it enters at the Splunk [data pipeline](https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Datapipeline)'s [indexing](https://docs.splunk.com/Documentation/Splunk/latest/Indexer/Howindexingworks) segment.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
> 
{.box .info}

## Configuring Cribl Stream to Output to Splunk HEC Destinations

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Destination** at right. From the resulting drawer's tiles, select **Splunk** > **HEC**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.

Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles or the **Destinations** left nav, select **Splunk** > **HEC**. Next, click **Add Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Splunk HEC definition.

**Load balancing**: When enabled (default), lets you specify multiple [Splunk HEC endpoints](#splunk-hec-endpoints) and load weights. With the default `No` setting, if you notice that Cribl Stream is not sending data to all possible IP addresses, enable **Advanced Settings** > **Round-robin DNS**.

**Splunk HEC endpoint**: URL of a Splunk HEC endpoint to send events to (e.g., `http://myhost.example.com:8088/services/collector/event`). This setting appears only when **Load balancing** is toggled to `No`.

#### Splunk HEC Endpoints {#splunk-hec-endpoints}

The **Splunk HEC Endpoints** table is where you specify a known set of receivers on which to load-balance data. It appears only when **Load balancing** is toggled to `Yes`.

Click **Add Endpoint** to specify more receivers on new rows. Each row provides the following fields:

**HEC Endpoint**: Specify the URL to a Splunk HEC [endpoint](https://docs.splunk.com/Documentation/Splunk/latest/Data/HECRESTendpoints) to send events to. See the Splunk [documentation]( https://docs.splunk.com/Documentation/Splunk/latest/Data/UsetheHTTPEventCollector#Configure_HTTP_Event_Collector_on_Splunk_Cloud) for details on getting your URL. For Splunk Cloud endpoints, `https` protocol is recommended. Supported endpoints include:
  * `/services/collector/event` (default)
  * `/services/collector/raw`
  * `/services/collector/health`
  * `/services/collector/s2s`

**Load weight**: Set each connection's relative traffic-handling capability by assigning a weight (> `0`). This column accepts arbitrary values, but for best results, assign weights in the same order of magnitude to all connections. Cribl Stream will attempt to distribute traffic to the connections according to their relative weights.

The final column provides an `X` button to delete any row from the table.

> When you first enable load balancing, or if you edit the load weight once your data is load–balanced, give the logic time to settle. The changes might take a few seconds to register.
>
{.box .info}

For details on configuring all these options, see [About Load Balancing](load-balancing).

> For Splunk Cloud endpoints, change the **Splunk HEC endpoint**'s default `http:` prefix to: `https:`.
>
{.box .success}

### Authentication Settings  {#authentication-settings}

Use the **Authentication method** buttons to select one of these options:

- **Manual**: Displays an **HEC Auth token** field for you to enter your Splunk HEC API access token. 

- **Secret**: This option exposes an **HEC Auth token (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-and-monitoring#secrets) that references the API access token described above. A **Create** link is available to store a new, reusable secret.

> This Destination does not support Mutual TLS (mTLS).
>
{.box .warning}

#### Indexer Acknowledgement
Do not set **Enable Indexer Acknowledgement** on the Splunk token. With this setting enabled, Splunk receivers expect the Channel GUID to be passed in, and requests will fail with errors of this form:

```
channel: output:splunk_cloud_hec
cid: w2
level: error
message: Request failed
reason: Received status code=400, method=POST, url=https://http-inputs-bazookatron.splunkcloud.com/services/collector/event
response: {"text":"Data channel is missing","code":10}
time: 2023-02-14T15:26:03.413Z
```

### Optional Settings

**Exclude current host IPs**: This toggle appears when **Load balancing** is set to `Yes`. It determines whether to exclude all IPs of the current host from the list of any resolved hostnames. Defaults to `No`, which keeps the current host available for load balancing.

**Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.   

**Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Stream's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

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

**Strict ordering**: The default `Yes` position enables FIFO (first in, first out) event forwarding. When receivers recover, Cribl Stream will send earlier queued events before forwarding newly arrived events. To instead prioritize new events before draining the queue, toggle this off. Doing so will expose this additional control:
- **Drain rate limit (EPS)**: Optionally, set a throttling rate (in events per second) on writing from the queue to receivers. (The default `0` value disables throttling.) Throttling the queue's drain rate can boost the throughput of new/active connections, by reserving more resources for them. You can further optimize Workers' startup connections and CPU load at **Group Settings** > [Worker Processes](settings-group-fleet#processes).

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Advanced Settings

**Output multi–metrics**: Toggle to `Yes` to output multiple-measurement metric data points. (Supported in Splunk 8.0 and above, this format enables sending multiple metrics in a single event, improving the efficiency of your Splunk capacity.)

**Validate server certs**: Reject certificates that are not authorized by a trusted CA (e.g., the system's CA). Defaults to `Yes`.

**Round–robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts. (This setting is available only when **General Settings** > **Load balancing** is set to `No`.)

**Compress**: Compresses the payload body before sending. Defaults to `Yes` (recommended).

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

**Extra HTTP headers**: Click **Add Header** to add **Name**/**Value** pairs to pass as additional HTTP headers. Values will be sent encrypted.

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


## Notes on HTTP-Based Outputs

- To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).

- Cribl Stream will attempt to use keepalives to reuse a connection for multiple requests. After two minutes of the first use, the connection will be thrown away, and a new connection will be reattempted. This is to prevent sticking to a particular Destination when there is a constant flow of events. 

- If the server does not support keepalives – or if the server closes a pooled connection while idle – a new connection will be established for the next request.

- When resolving the Destination's hostname with load balancing disabled, Cribl Stream will pick the first IP in the list for use in the next connection. Enable Round-robin DNS to better balance distribution of events between Splunk HEC servers.

* See [Splunk's documentation](https://docs.splunk.com/Documentation/SplunkCloud/latest/Data/Configureindex-timefieldextraction#Add_an_entry_to_fields.conf_for_the_new_field) on editing `fields.conf` to ensure the visibility of index-time fields sent to Splunk by Cribl Stream.

## Troubleshooting Resources {#troubleshooting}

[Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Destination Integrations: Splunk Cloud](https://university.cribl.io/#/online-courses/7d6db44d-622f-4af4-8edb-429be2ebcc28) short course. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
