# New Relic Events Destination


Cribl Stream supports sending events to New Relic via the [New Relic Event API](https://docs.newrelic.com/docs/telemetry-data-platform/ingest-apis/introduction-event-api/). Use this Destination to export ad hoc events that New Relic ingestion treats as [custom events](https://developer.newrelic.com/collect-data/custom-events/). 

To export structured log and/or metric events, use Cribl Stream's [New Relic Logs & Metrics](destinations-newrelic) Destination.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info} 

## Configure Cribl Stream to Output Events to New Relic

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Destination. 
   - **Description**: Optionally, enter a description.
   - **Region**: Select which New Relic region endpoint to use. Select `Custom` to manually enter your New Relic region’s URL if it’s not listed.
   - **Account ID**: Enter your New Relic account ID. (You can access this ID from New Relic's **account** drop-down, by selecting **Manage your plan**.)
   - **Event type**: Default `eventType` to apply when not specified in an event. You can use arbitrary values, as long as they do not conflict with New Relic [reserved words](https://docs.newrelic.com/docs/telemetry-data-platform/custom-data/custom-events/data-requirements-limits-custom-event-data/#prohibited-eventType). (Where an `eventType` is specified in an event, it will override this value.)
3. Under **Authentication**, select an **Authentication method** from the dropdown:
    > If an incoming event contains an internal field named `__newRelic_apiKey`, the New Relic Events Destination will use that field's value as the API key when sending the event to New Relic. 
    > 
    > For events that do not contain a `__newRelic_apiKey` field, the Destination will use whatever API key you have configured in this section.
    >
    {.box .info}

    - **Manual**: This default option exposes an **API key** field. Directly enter your New Relic Ingest License API key, as you created or accessed it from New Relic's **account** drop-down. (For details, see the [New Relic API Keys](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#ingest-license-key) documentation.)

   - **Secret**: This option exposes an **API key (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references a New Relic Ingest License API key. A **Create** link is available to store a new, reusable secret.
4. Next, you can configure the following **Optional Settings**: 
    - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`. For the `Persistent Queue` option, see the section just below.
    - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page.. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can adjust the [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries), and [Advanced](#advanced-tab) settings outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**. 

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.10/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}
 
#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out via this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Retries {#retries}

[Snippet not found: content/shared/4.10/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Toggle on to reject certificates that are **not** authorized by a CA in the **CA certificate path**, nor by another trusted CA (for example, the system's CA).

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Compress**: Defaults to toggled on, meaning compress the payload body before sending.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Stream hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Max body size (KB)**: Maximum size of the request body before compression. Defaults to `1024` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Max body size** value until downstream receivers reliably accept all events.

**Max events per request**: Maximum number of events to include in the request body. Defaults to `0`,  allowing unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values can cause the payload size to be smaller than the configured **Max body size**. Defaults to `1` second.

**Extra HTTP headers**: Click **Add Header** to insert extra headers as **Name**/**Value** pairs. Values will be sent encrypted.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Stream will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Verifying the New Relic Events Destination

Once you've configured event sources, create one or more Routes to send data to New Relic. 

In New Relic, you can create visualizations incorporating the Cribl Stream-supplied data, then add them to new or existing dashboards as widgets.

Alternatively, in the New Relic backend, you can select **Query you data** (top nav) > **Events**  (left tab), and then select the event type you exported from Cribl Stream. 

To view more events, change the time frame at the upper right. To see raw events, click **Raw data** on the right.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).



## Troubleshooting

[Snippet not found: content/shared/4.10/snippets/_destinations-troubleshooting.md]