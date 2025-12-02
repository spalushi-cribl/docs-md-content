# New Relic Logs & Metrics Destination


Cribl Stream supports sending events to the [New Relic Log API](https://docs.newrelic.com/docs/logs/log-management/log-api/introduction-log-api) and the [New Relic Metric API](https://docs.newrelic.com/docs/telemetry-data-platform/ingest-manage-data/ingest-apis/report-metrics-metric-api).

> Type: Streaming | TLS Support: Yes | PQ Support: Yes
> 
> In Cribl Stream 3.1.2 and newer, this Destination now authenticates using New Relic's [Ingest License API key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#ingest-license-key). (New Relic will retire the Insights Insert API keys, which this Destination previously used for authentication.)
> 
> Also in v.3.1.2 and newer, Cribl Stream provides a separate [New Relic Events](destinations-newrelic-events) Destination that you can use to send ad hoc (loosely structured) events to New Relic via the [New Relic Event API](https://docs.newrelic.com/docs/telemetry-data-platform/ingest-apis/introduction-event-api/).
> 
> Within New Relic's platform, you can monitor Cribl Stream's performance and data flow by installing [New Relic's Cribl dashboard](https://newrelic.com/instant-observability/cribl-logstream/e67f2859-80c1-4234-bbcf-bcbeeb31d70d?utm_source=external_partners&utm_medium=referral&utm_campaign=global-ever-green-io-partner).
>
{.box .info} 

## Configure Cribl Stream to Output to New Relic

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   **Output ID**: Enter a unique name to identify this New Relic definition. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**.  
   - **Description**: Optionally, enter a description.
   - **Region**: Select which New Relic region endpoint to use. Select `Custom` to manually enter your New Relic region’s URL if it’s not listed.
3. Under **Authentication**, select an **Authentication method** from the dropdown:
    > If an incoming event contains an internal field named `__newRelic_apiKey`, the New Relic Logs & Metrics Destination uses that field's value as the API key when sending the event to New Relic. 
    > 
    > For events that do not contain an `__newRelic_apiKey` field, the Destination uses whatever API key you have configured in the **Authentication method** settings.
    >
    {.box .info}

   - **Manual**: This default option exposes an **API key** field. Directly enter your New Relic Ingest License API key, as you created or accessed it from New Relic's **account** drop-down. (For details, see the [New Relic API Keys](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#ingest-license-key) documentation.)

   - **Secret**: This option exposes an **API key (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references a New Relic Ingest License API key. A **Create** link is available to store a new, reusable secret.
4. Next, you can configure the following **Optional Settings**: 
   
    - **Log type**: Name of the `logType` to send with events. For example, `observability` or `access_log`.
        > This sets a default. Where a `sourcetype` is specified in an event, it will override this value.
        >
        {.box .info}
    - **Log message field**: Name of the field to send as the log `message` value. If not specified, the event will be serialized and sent as JSON.
    - **Fields**: Additional fields to (optionally) add, as **Name**-**Value** pairs. Click **Add Field** to add more. For details, see [Fields]{#fields}
    - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`. For the `Persistent Queue` option, see the section just below.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-tab) settings outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**. 

### Fields {#fields}
These custom fields allow you to enrich your log data with specific details that are relevant to your use case. Click **Add Field** for more.

- **Name**: Enter the field name.

- **Value**:JavaScript expression to compute field’s value, enclosed in single quotes, double quotes, or backticks. (Can evaluate to a constant.)

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.12/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Retries {#retries}

[Snippet not found: content/shared/4.12/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Toggle on to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (for example, the system's CA).

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Compress**: Toggle on (default) to compress the payload body before sending.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Stream hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `1024` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Buffer memory limit (KB)**: Total amount of memory used to buffer outgoing requests waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced. This provides granular control over the memory allocated for buffering outgoing batched requests. Increasing the limit allows batches to grow larger before being flushed, improving efficiency for data with high cardinality (a large number of unique batches). Finding the optimal balance between efficient data transfer and memory usage involves adjusting both the **Buffer memory limit** and **Body size limit** settings.

**Events-per-request limit**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values can cause the payload size to be smaller than the configured **Body size limit**. Defaults to `1` second.

**Extra HTTP headers**: Click **Add Header** to insert extra headers as **Name**/**Value** pairs. Values will be sent encrypted.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Stream will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Verifying the New Relic Destination

Once you've configured log and/or metrics sources, create one or more Routes to send data to New Relic. 

In New Relic, you can create visualizations incorporating the Cribl Stream-supplied data, then add them to new or existing dashboards as widgets.

Logs and metrics land in two different places in New Relic.

### Log Queries

To access and query log data:

- Navigate to the New Relic home screen's **Logs** header option, and click the **(+)** button at right.

- Then to build your queries, use the **Find logs where** input field, and add desired columns to the table view below the graph,.

### Metrics Queries

To access and query metrics data:

- From the New Relic home screen, *Click **Browse Data** > **Metrics** > **Can Search for metricNames**.

- Then, customize time range and dimensions to build the desired logic for your queries.

- Alternatively, you can use [NRQL](https://developer.newrelic.com/collect-data/query-data-nrql/) to build your own query searches.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting

[Snippet not found: content/shared/4.12/snippets/_destinations-troubleshooting.md]