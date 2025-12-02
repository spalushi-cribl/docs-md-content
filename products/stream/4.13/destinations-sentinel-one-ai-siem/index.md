# SentinelOne AI SIEM Destination


The **SentinelOne AI SIEM** Destination sends data to SentinelOne’s Singularity AI SIEM platform using the HTTP Event Collector (HEC) protocol. 

> Type: Streaming | TLS Support: Yes | PQ Support: No 
> 
{.box .info}

## Configure a SentinelOne AI SIEM Destination

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
1. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Destination. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**. 
   - **Description**: Optionally, enter a description.
   - **Region**: Select the geographical region for your SentinelOne AI SIEM ingestion endpoint. This must match your tenant's deployment to ensure correct data routing and compliance.
     - **US** – `United States: https://ingest.us1.sentinelone.net`
     - **CA** – `Canada: https://ingest.ca1.sentinelone.net`
     - **EMEA** – `Europe, Middle East, and Africa: https://ingest.eu1.sentinelone.net`
     - **AP** – `India: https://ingest.ap1.sentinelone.net`
     - **APS** – `India (South): https://ingest.aps1.sentinelone.net`
     - **AU** – `Australia: https://ingest.apse2.sentinelone.net`
     - **Custom Tenant**: Enter your **Base AI SIEM endpoint URL** in this format: 
       - `https://<Your-S1-Tenant>.sentinelone.net`. 
       - The URL must begin with `http://` or `https://`, can include a port number, must not have a trailing slash, and match this pattern: `^https?://[a-zA-Z0-9.-]+(:[0-9]+)?$`.
   - **AI SIEM endpoint path**: Select the HTTP API route used to send events to SentinelOne AI SIEM. This choice determines the expected data format and how custom fields are handled.
     - `/services/collector/event` (Default): Select this path to send structured JSON payloads with standard HEC top-level fields. The **Event Fields** tab is available to configure metadata fields. Standard HEC fields are evaluated and set as top-level fields in the outgoing JSON payload. Custom metadata fields are included under the `fields` object.
     - `/services/collector/raw`: Select this path to send raw, unstructured log lines (plain text). The **Raw Fields** tab is available to configure metadata fields. The fields are sent as query parameters in the request URL.
1. Under **Authentication**, select an **Authentication method** from the dropdown:
    - **Manual**: Displays an **AI SIEM API Key** field for you to enter the API key string used to authenticate requests to the SentinelOne AI SIEM endpoint. This key is included in the `Authorization` header as a Bearer token for each HTTP request sent.
    - **Secret**: This option exposes an **AI SIEM API Key (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references the API key. A **Create** link is available to store a new, reusable secret.

    > This Destination does not support Mutual TLS (mTLS).
    {.box .warning}
1. Next, you can configure the following **Optional Settings**:
   - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.   
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. Depending on the **AI SIEM endpoint path** you selected, you can configure either **Event Fields** or **Raw Fields**. Fields have a default expression that you can adjust for dynamic data extraction or static assignment of values.
   > **Event Fields** are JavaScript expressions and **Raw Fields** are string expressions except for **serverHost** which is a JavaScript expression. In JavaScript expressions, you must enclose text constants in single quotes, like `'security'`. Enter string expressions directly without quotes, spaces are allowed.
   {.box .info}
   | Field | Event Default | Raw Default |
   | --- | --- | --- |
   | **serverHost** | `__e.host \|\| C.os.hostname()` | `C.os.hostname()` |
   | **logFile** | `__e.source \|\| (__e.__criblMetrics ? 'metrics' : 'cribl')` | `cribl` |
   | **parser** | `__e.sourcetype \|\| 'dottedJson'` | `hecRawParser` |
   | **dataSource.category** | `'security'` | `security` |
   | **dataSource.name** | `__e.__dataSourceName \|\| 'cribl'` | `cribl` |
   | **dataSource.vendor** | `__e.__dataSourceVendor \|\| 'cribl'` | `cribl` |
   | **event.type** | | |
1. Optionally, you can adjust the [Processing](#processing), [Retries](#retries) and [Advanced](#advanced-tab) settings outlined in the sections below.
1. Select **Save**, then **Commit & Deploy**. 

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.13/snippets/_destinations-persistent-queue-settings.md]

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

[Snippet not found: content/shared/4.13/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Reject certificates that are not authorized by a trusted CA (for example, the system's CA). Defaults to toggled on.

**Compress**: Toggle on (default) to compress the payload body before sending.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Stream hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`. Each request can potentially hit a different HEC receiver.

**Body size limit (KB)**: Maximum size, in KB, of the request body. Defaults to `5120`. Lowering the size can potentially result in more parallel requests and also cause outbound requests to be made sooner.

**Events-per-request limit**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**: Maximum time between requests. Low values can cause the payload size to be smaller than the configured **Body size limit**. Defaults to `5`.

> * Retries happen on this flush interval. 
> * Any HTTP response code in the `2xx` range is considered success. 
> * Any response code in the `5xx` range is considered a retryable error, which will not trigger persistent queue (PQ) usage.
> * Any other response code will trigger PQ (if PQ is configured as the Backpressure behavior).
{.box .info}

**Extra HTTP headers**: Select **Add Header** to add **Name**/**Value** pairs to pass as additional HTTP headers. Values will be sent encrypted.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Stream will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Notes on HTTP-Based Outputs

- To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).

- Cribl Stream will attempt to use keepalives to reuse a connection for multiple requests. After two minutes of the first use, the connection will be thrown away, and a new connection will be reattempted. This is to prevent sticking to a particular Destination when there is a constant flow of events. 

- If the server does not support keepalives – or if the server closes a pooled connection while idle – a new connection will be established for the next request.

## Troubleshooting {#troubleshooting}

[Snippet not found: content/shared/4.13/snippets/_destinations-troubleshooting.md]
