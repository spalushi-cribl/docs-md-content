# Google Security Operations (SecOps) Destination


Cribl Edge supports sending data to [Security Operations (SecOps)](https://cloud.google.com/chronicle/docs/overview), a cloud service for retaining, analyzing, and searching enterprise security and network telemetry data. This service was [formerly known as Google Chronicle](https://www.googlecloudcommunity.com/gc/SIEM-Forum/Chronicle-Security-Operations-has-been-rebranded-to-Google/m-p/744939).

if you plan to use **V2** of the Google Security Operations API, you need your [Google service account credentials](https://cloud.google.com/chronicle/docs/reference/ingestion-api#getting_api_authentication_credentials) to define a Google SecOps Destination.

If you plan to use **V1** of the Google Security Operations API, Cribl recommends using Google service account credentials for authentication, but alternatively, you can opt to use an API key from Google.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info}

## Options for Sending Data to SecOps {#udm-vs-unstructured}

When sending data to SecOps, you have two primary methods, each with its own advantages:

* **Sending Unstructured Data for SecOps Parsing**: This is ideal for well-known data types. You send your raw, unstructured data, and SecOps uses its built-in parsers to transform it into the [Unified Data Model](https://cloud.google.com/chronicle/docs/unified-data-model/format-events-as-udm) (UDM). However, this approach can lead to parsing errors and troubleshooting if you're dealing with less common or proprietary data types.

* **Sending Pre-Structured UDM Data**: This method bypasses SecOps's parsing process entirely. It's especially beneficial for less-usual or unique, proprietary data types that might confuse SecOps's parsers. By sending data that's already structured as UDM, you gain greater control and flexibility over how your data lands in SecOps. The trade-off is that you "own" the data transformation process, using Pipelines and Functions to convert your initial unstructured data to UDM before sending it to SecOps.

   > Do not place UDM fields like `metadata`, `principal`, and `network` inside `_raw`. These fields must exist as top-level fields in the event payload.
   {.box .warning}

### Working with Unstructured Log Types {#log-types}

Google SecOps supports a dynamic list of unstructured log types that evolves over time. There are two configuration settings for managing log type assignments:

* **Default log type**: Cribl Edge periodically updates its **Default log type** dropdown with the latest list from Google.
* **Custom log types**: If a log type you need isn't in the default dropdown, or if you're using an older, removed log type, you can define and manage it in the **Custom log types** table under **Optional Settings**.

For Google's latest list of supported log types, see the [Google SecOps SIEM reference](https://cloud.google.com/chronicle/docs/ingestion/parser-list/supported-default-parsers) or send a `GET` request to `https://malachiteingestion-pa.googleapis.com/v1/logtypes`.

#### How Log Types are Assigned

* **Event-specific**: When processing an event, Cribl Edge first checks if the event has a `__logType` field. If this field is present, its value determines the log type for that event.
* **Default**: If the event lacks a `__logType` field, Cribl Edge assigns the log type specified in the **Default log type** drop-down.
* **Custom**: If the **Default log type** is set to `Custom`, Cribl Edge will use the first custom log type listed in the **Custom log types** table. Reorder the custom log types in the table using the grab handles to prioritize which custom log type should be used as the default. 
  * If the **Default log type** is set to a specific log type (not `Custom`), the custom log types defined in the **Custom log types** table will not be used as defaults. However, you can still use these custom log types by explicitly setting the `__logType` field in your events to one of the custom log types.

## Configure Cribl Edge to Output to SecOps

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this SecOps output definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Send events as**: From the drop-down, choose how you want the Destination to send data to SecOps. The options are: **Unstructured** and **UDM**. See [Options for Sending Data to SecOps](#udm-vs-unstructured) below to learn about the differences between working with `Unstructured` and already-structured `UDM` data.
   - **API version**: The version of the [Google Security Operations Ingestion API](https://cloud.google.com/chronicle/docs/reference/ingestion-api) you want the Destination to use when communicating with SecOps. Defaults to `V2`. 
   - **Default log type**: This setting is available only when **Send events as** is set to `Unstructured` (the default). From the drop-down, select a log type to assign to events that lack a `__logType` field. For any events that **do** have a `__logType` field, Cribl Edge uses the value in that field, not the default log type. See [Working with Log Types](#log-types) below to learn how Cribl Edge populates the drop-down and how to add log types missing from it.

      When **API version** is set to `V2` (the default), one or both of the following additional controls appear in **General Settings**:

    - **Namespace**: This setting is available only when **Send events as** is set to `Unstructured` (the default). Optionally, configure an environment namespace to identify logs' originating data domain. You can use this as a tag when indexing and/or enriching data. The `__namespace` event field, if present, will overwrite this.  
    - **Customer ID**: A unique identifier (UUID) corresponding to a particular SecOps instance. To use this optional field, request the ID from your SecOps representative.  <br>
  
      When **API version** is set to `V1` and **Send events as** is set to `Unstructured`, both **Namespace** and **Customer ID** appear under **Optional Settings**.  <br>

3. Next, you can configure the following **Optional Settings**: When **General Settings** > **Send events as** is set to `Unstructured` (the default), the UI displays some or all of these settings, depending on how **API version** is set.
      - **Custom log types**: For each log type you want to define, click **Add type** and enter values for **Log type** and **Description** in the resulting table row. See [Working with Log Types](#log-types) above for more details.
      - **Namespace**: Appears here when **API version** is set to `V1`. Described in **General Settings** above.
      - **Customer ID**: Appears here when **API version** is set to `V1`. Described in **General Settings** above.
      - **Custom labels**: You can (optionally) define labels for the Destination to add to events. These "custom labels" are a Google SecOps feature used for data role-based access control (data RBAC) and other purposes. See the [Google SecOps documentation](https://cloud.google.com/chronicle/docs/preview/cloud-integration/create-custom-labels). Click **Add label** to create a new label and specify it as a key-value pair.
      - **Log text field**: Specify the event field that contains the log text to send. If you do not specify a log text field, Cribl Edge sends a JSON representation of the whole event.</br> </br> 

         (The following settings are available regardless of how **General Settings > Send events as** is set.) </br> </br> 
     
     -  **Region**: The Google SecOps regional endpoint to send events to.
     -  **Backpressure behavior**: Whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`. 
     -  **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Authentication](#auth), [Persistent Queue](#pq), [Processing](#processing), [Retries](#retries) and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**. 

### Authentication {#auth}

To complete this part of the Destination definition, you will need either a Google Security Operations API key or your Google SecOps service account credentials. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.

Use the **Authentication method** drop-down to select an option. Whether you have set **General Settings > API version** to `V2` or `V1` determines what options are displayed.

For API version `V2`, the options are: 

- **Service Account Credentials**: This option exposes a field where you can enter your Google SecOps service account directly.

- **Service Account Credentials Secret**: This option exposes a drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references your Google SecOps service account. A **Create** link is available to store a new, reusable secret.

For API version `V1`, the options include the two listed above plus two more:

- **API key**: This option exposes a field where you can enter your Google Security Operations API key.

- **API Key Secret**: This option exposes a drop-down, in which you can select a [stored secret](/stream/securing-secrets) that references your Google Security Operations API key. A **Create** link is available to store a new, reusable secret.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.12/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post-Processing

**Pipeline**: In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

**System fields**: Specify any fields you want Cribl Edge to automatically add to events using this output. Wildcards are supported. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Retries {#retries}

**Honor Retry-After header**: Whether to honor a [`Retry-After` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After), provided that the header specifies a delay no longer than 180 seconds. Cribl Edge limits the delay to 180 seconds even if the `Retry-After` header specifies a longer delay. When enabled, any `Retry-After` header received takes precedence over all other options configured in the **Retries** section. When disabled, all `Retry-After` headers are ignored.

**Settings for failed HTTP requests**: When you want to automatically retry requests that receive particular HTTP response status codes, use these settings to list those response codes. 

For any HTTP response status codes that are not explicitly configured for retries, Cribl Edge applies the following rules:

| Status Code | Action |
| --- | --- |
| Any in the `1xx`, `3xx`, or `4xx` series | Drop the request |
| Any in the `5xx` series                  | Retry the request |

Upon receiving a response code that's on the list, Cribl Edge first waits for a set time interval called the **Pre-backoff interval** and then begins retrying the request. Time between retries increases based on an [exponential backoff algorithm](https://en.wikipedia.org/wiki/Exponential_backoff) whose base is the **Backoff multiplier**, until the backoff multiplier reaches the **Backoff limit (ms)**. At that point, Cribl Edge continues retrying the request without increasing the time between retries any further.

If the sender (which manages the connection to the Destination) is at capacity, it will not accept any incoming events. These incoming events originate internally from a previous stage of the data flow when Destinations send outbound requests to their respective external services, and they include retry requests and new requests. Any events that were already in transit when the sender reached capacity will continue to be processed downstream.

Sender capacity is freed up when an outgoing request succeeds or encounters a non-retryable error. When the sender has available capacity again, it will resume accepting incoming events. This capacity management is influenced by the number of active connections and configured limits, such as concurrency and buffer sizes. If a Pipeline sends events faster than the Destination can process, the buffers may fill up, leading to backpressure and `Sender at capacity` warnings. This backpressure prevents the sender from accepting additional requests until capacity is restored.

By default, `429 (Too Many Requests)` and `503 (Service Unavailable)` are the only response codes configured for automatic retries. For each response code you want to add to the list, click **Add Setting** and configure the following settings: 

- **HTTP status code**: A response code that indicates a failed request, for example `429 (Too Many Requests)` or `503 (Service Unavailable)`.
- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `30000` (30 seconds).
- **Backoff multiplier**: The base for the exponential backoff algorithm; defaults to `1`. A value of `2` means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Edge should apply for its final retry, in milliseconds. Defaults to `30000` (30 seconds); minimum is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

**Retry timed-out HTTP requests**: When you want to automatically retry requests that have timed out, toggle this control on to display the following settings for configuring retry behavior:

- **Pre-backoff interval (ms)**: The amount of time to wait before beginning retries, in milliseconds. Defaults to `1000` (one second).
- **Backoff multiplier**: The base for the exponential backoff algorithm. A value of `2` (the default) means that Cribl Edge will retry after 2 seconds, then 4 seconds, then 8 seconds, and so on.
- **Backoff limit (ms)**: The maximum backoff interval Cribl Edge should apply for its final retry, in milliseconds. Default (and minimum) is `10,000` (10 seconds); maximum is `180,000` (180 seconds, or 3 minutes).

### Advanced Settings {#advanced-tab}

**Validate server certs**: Reject certificates that are **not** authorized by a CA in the **CA certificate path**, or by another trusted CA (such as the system's CA). Defaults to toggled on.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Edge will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

**Compress**: Toggle on (default) to compress the payload body before sending.

**Request timeout**: Enter an amount of time, in seconds, to wait for a request to complete before aborting it. Defaults to `90`.

**Request concurrency**: Enter the maximum number of ongoing requests to allow before blocking.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `1024` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Buffer memory limit (KB)**: Total amount of memory used to buffer outgoing requests waiting to be sent. If left blank, defaults to five times the max body size (if set). If `0`, no limit is enforced. This provides granular control over the memory allocated for buffering outgoing batched requests. Increasing the limit allows batches to grow larger before being flushed, improving efficiency for data with high cardinality (a large number of unique batches). Finding the optimal balance between efficient data transfer and memory usage involves adjusting both the **Buffer memory limit** and **Body size limit** settings.

**Events-per-request limit**: Enter the maximum number of events to include in the request body. Defaults to `0` (unlimited).

**Flush period (sec)**: Enter the maximum time to allow between requests. Be aware that small values could cause the payload size to be smaller than the configured **Body size limit**.

**Extra HTTP Headers**: Click **Add Header** to insert extra headers as **Name/Value** pairs. Values will be sent encrypted.

**Failed request logging mode**: Use this drop-down to determine which data should be logged when a request fails. Select among `None` (the default), `Payload`, or `Payload + Headers`. With this last option, Cribl Edge will redact all headers, except non-sensitive headers that you declare below in **Safe headers**. 

**Safe headers**: Add headers to declare them as safe to log in plaintext. (Sensitive headers such as `authorization` will always be redacted, even if listed here.) Use a tab or hard return to separate header names.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting

[Snippet not found: content/shared/4.12/snippets/_destinations-troubleshooting.md]