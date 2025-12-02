# Cortex XSIAM Destination


The **Cortex XSIAM** Destination allows you to seamlessly stream event data to Palo Alto Networks' [Cortex XSIAM](https://docs-cortex.paloaltonetworks.com/p/XSIAM) (Extended Security Intelligence and Automation Management) for advanced threat detection and response.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
> 
{.box .info}

**XSIAM Data Requirements** 

Cribl Edge provides a generic **Palo Alto XSIAM** Pack designed to extract and map mandatory fields. Cribl Edge automatically validates and converts these fields into HTTP headers, ensuring compliance with XSIAM requirements. For details on using the Pack, see [how to map data to the XSIAM Destination](#map-data).



**Optimized Data Delivery**:

- **Automatic Format Parsing**: Detects event formats, including CEF, LEEF, Syslog, JSON, and Raw.
- **Batched Transmission**: To optimize data ingestion and reduce overhead, events are grouped into batches. Set the [**Flush period**](#flush) to control latency. Allowed values are `1` second or greater.
- **Aggregation**: Uses batched keys derived from the event format and system fields.
- **Payload Size**: Enforces a 5 MB limit per individual event and a 10 MB limit per batch. Warnings are logged if data exceeds these limits.
- **Rate Limiting**: There's a default request rate limit of `400` requests per second. Customizable with the [**Throttle request rate limit**](#rrl) setting to align with your XSIAM capacity.
- **JSON formatting**: Events are serialized into JSON format for XSIAM ingestion.
  - The `Content-Type: application/json` header is included in all requests.
  - The `data` field is represented as a native JSON object, ensuring direct parsing of its contents. This native JSON structure allows XSIAM to efficiently query and analyze the data within the field, for example: 
  ```json
  {"data": {"resource": "/done", "path": "/done", "isBase64Encoded": false}, "collector_ms": 1742743457851}
  ```

## Prerequisite {#prerequisite}

**In XSIAM**, create a data collector instance and obtain an authorization token and endpoint URL. See the appropriate Cortex XSIAM documentation below for instructions.

   * [Cortex XSIAM 2.x](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-Documentation/Ingest-data-from-Cribl)
   * [Cortex XSIAM Premium](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-Premium-Documentation/Ingest-data-from-Cribl)
   * [Cortex XSIAM Enterprise](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-Enterprise-Documentation/Ingest-data-from-Cribl)
   * [Cortex XSIAM NG SIEM](https://docs-cortex.paloaltonetworks.com/r/Cortex-XSIAM/Cortex-XSIAM-NG-SIEM-Documentation/Ingest-data-from-Cribl)

## Configure an XSIAM Destination

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
1. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **XSIAM endpoint**: XSIAM endpoint URL to send events to, following this format `https://api-{tenant external URL}/logs/v1/event`. Defaults to `http://localhost:8088/logs/v1/event`.
1. Under **Authentication**, select an **Authentication method** from the dropdown:
    - **Bearer Token**: Displays an **Auth token** field for you to enter your XSIAM authorization token.
    - **Secret**: Displays an **Auth token (text secret)** drop-down to select a [stored secret](securing-secrets). A **Create** link is available to store a new, reusable secret.
1. Next, you can configure the following **Optional Settings**:
   - **Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting [backpressure](manage-backpressure). (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.   
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. If you assigned the **Backpressure behavior** to `Persistent Queue` you can adjust the [Persistent Queue](#pq) settings.
1. Optionally, you can adjust the [Retries](#retries), and [Advanced](#advanced-tab) settings outlined in the sections below.
1. Select **Save**.
1. [Map data](#map-data) to the XSIAM Destination.
1. **Commit & Deploy** your changes.

### Map data to the XSIAM Destination {#map-data}

**In Cribl Edge**, add the [**Palo Alto XSIAM** Pack](https://packs.cribl.io/packs/cribl-paloalto-xsiam) to your Fleet. Navigate to **Processing** > **Packs** from the Fleet submenu, and [add the Pack](packs#get-packs). Once added, you'll be able to assign the Pack to your [Route](routes) or [QuickConnect](quickconnect) that maps data from Sources to the XSIAM Destination.
   * The Pack includes several Pipelines designed for specific data types. 
      > Configure the Pack **Route** to use the appropriate **Pipeline** for your data type.
      {.box .warning}
   * If you're sending different data types for your XSIAM Destination, create a separate Pack with the appropriate Pipeline for each data type.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.12/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post‑Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries {#retries}

[Snippet not found: content/shared/4.12/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Reject certificates that are not authorized by a trusted CA (for example, the system's CA). Defaults to toggled on.

**Round–robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30`.

**Request concurrency**: Maximum number of concurrent requests per Worker Process. When Cribl Edge hits this limit, it begins throttling traffic to the downstream service. Defaults to `5`. Minimum: `1`. Maximum: `32`.

**Events-per-request limit**: Maximum number of events to include in the request body. The `0` default allows unlimited events.

**Flush period (sec)**:{{< id flush >}} Maximum time between requests. Low values can cause the payload size to be smaller than the configured **Body size limit**. Defaults to `1`.

> * Retries happen on this flush interval. 
> * Any HTTP response code in the `2xx` range is considered success. 
> * Any response code in the `5xx` range is considered a retryable error, which will not trigger Persistent Queue (PQ) usage.
> * Any other response code will trigger PQ (if PQ is configured as the **Backpressure behavior**).
>
{.box .info}

**Throttle request rate limit**:{{< id rrl >}} Maximum number of HTTP requests sent per second. Defaults to `400`, maximum `2000`. Requests exceeding this limit are stored in the persistent queue until capacity becomes available.

**Extra HTTP headers**: Click **Add Header** to add **Name**/**Value** pairs to pass as additional HTTP headers. Values will be sent encrypted.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Notes on HTTP-Based Outputs

- To proxy outbound HTTP/S requests, see [System Proxy Configuration](proxy-config).

- Cribl Edge will attempt to use keepalives to reuse a connection for multiple requests. After two minutes of the first use, the connection will be thrown away, and a new connection will be reattempted. This is to prevent sticking to a particular Destination when there is a constant flow of events. 

- If the server does not support keepalives – or if the server closes a pooled connection while idle – a new connection will be established for the next request.

## Troubleshooting {#troubleshooting}

This table outlines common API call errors, showing response codes and log messages for troubleshooting.

| Scenario | Response Code | Response Body |
| ---  | --- | --- |
| Unauthorized access | `401` | <div style="width: 280px">N/A (Typically no body for a `401` response)</div> |
| No Source-Identifier header or an invalid UUID provided in the Source-Identifier header | `400` | `missing or unknown UUID was signaled in Source-Identifier header` |
| The “Other” (non-XSIAM) integration UUID (`af01292940d7426594d3d3e55ae17ee0`) was signaled in the Source-Identifier but the Vendor and/or Product headers are missing or empty. | `400` | `must signal both vendor and product for non-xsiam integrations` |
| Missing Format header | `400` | `missing format header` |
| Missing Integration-Identifier header | `400` | `missing Integration-Identifier header` |

### Configuration Modal

[Snippet not found: content/shared/4.12/snippets/_destinations-troubleshooting.md]
