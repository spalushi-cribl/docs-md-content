# Google Cloud Chronicle API Destination


The **Google Cloud Chronicle API** Destination sends data to Google Cloud Chronicle using the v1alpha [`ImportLogs`](https://cloud.google.com/chronicle/docs/reference/ingestion-methods#importlogs) ingestion method.
The Destination sends your raw, unstructured data, and Chronicle API uses its built-in parsers to transform it into the [Unified Data Model](https://cloud.google.com/chronicle/docs/unified-data-model/format-events-as-udm) (UDM).

Using the **Google Cloud Chronicle API** Destination, in contrast to the [Google Security Operations (SecOps) Destination](destinations-google_chronicle),
lets you add labels to individual events, not only at the batch level,
and take advantage of the improved performance and scaling offered by Chronicle API.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
> 
{.box .info}

### Working with Unstructured Log Types {#log-types}

Google Cloud Chronicle API supports a dynamic list of unstructured log types that evolves over time.

The **Default log type** setting lets you select the log type to send.
To send a log of an unsupported type, type your custom log type into this field.

For Google's latest list of supported log types, see the [Supported log types and default parsers](https://cloud.google.com/chronicle/docs/ingestion/parser-list/supported-default-parsers) in Google Cloud documentation to learn more about the specific log types.

#### How Log Types are Assigned

* **Event-specific**: When processing an event, Cribl Stream first checks if the event has a `__logType` field. If this field is present, its value determines the log type for that event.
* **Default**: If the event lacks a `__logType` field, Cribl Stream assigns the log type specified in the **Default log type** drop-down.
* **Custom**: If you entered a custom log name into **Default log type**, Cribl Stream will use this type.

## Configure a Google Cloud Chronicle API Destination

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
1. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Destination. If you clone this Destination, Cribl Stream will add `-CLONE` to the original **Output ID**. 
   - **Description**: Optionally, enter a description.
   - **Default log type**: Select or type in the default log type value to assign to events that lack a `__logType` field. See [Working with Unstructured Log Types](#log-types) for more information.
   - **GCP project ID**: Enter the Google Cloud Platform (GCP) project ID to send events to. You can find the project ID on the Google Cloud [Welcome page](https://console.cloud.google.com/welcome).
   - **GCP instance**: Enter ID of the GCP instance to send events to. This is the Chronicle customer UUID.
   - **Region**: Select the [regional endpoint](https://cloud.google.com/chronicle/docs/reference/rest#regional-service-endpoint) to send events to.
   - **Log text field**: Optionally, select the name of the event field that contains the log text to send. If not specified, Cribl Stream sends a JSON representation of the whole event.
1. Next, you can configure the following **Optional Settings**:
   - **Namespace**: User-configured environment namespace to identify the data domain from which the logs originated. This namespace is used as a tag to identify the appropriate data domain for indexing and enrichment functionality. The `__namespace` event field, if present, will overwrite this.  
   - **Custom labels**: Custom labels to be added to every event. Custom labels are a Google SecOps feature used for data role-based access control (data RBAC) and other purposes. See the [Google SecOps documentation](https://cloud.google.com/chronicle/docs/administration/configure-datarbac-users#create-manage-custom-labels). Click **Add label** to create a new label and specify it as a key-value pair.
   - **Backpressure behavior**: How to handle events when all receivers are exerting backpressure.
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
1. Under **Authentication**, select an **Authentication method** from the dropdown:
   - **Service Account Credentials**: This option exposes a field where you can enter your Google SecOps service account directly.
   - **Service Account Credentials Secret**: This option exposes a drop-down, in which you can select a [stored secret](securing-secrets) that references your Google SecOps service account. A **Create** link is available to store a new, reusable secret.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.14/snippets/_destinations-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Post-Processing

**Pipeline**: [Pipeline](pipelines) or [Pack](packs) to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Stream Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Stream Node that processed the event.
- `cribl_input` – Cribl Stream Source that processed the event.
- `cribl_output` – Cribl Stream Destination that processed the event.
- `cribl_route` – Cribl Stream Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Stream Worker Process that processed the event.

### Retries {#retries}

[Snippet not found: content/shared/4.14/snippets/_destinations-http-retries.md]

### Advanced Settings {#advanced-tab}

**Validate server certs**: Reject certificates that are not authorized by a trusted CA (for example, the system's CA). Defaults to toggled on.

**Round-robin DNS**: Toggle on to enable round-robin DNS lookup across multiple IP addresses, IPv4 and IPv6. When a DNS server resolves a Fully Qualified Domain Name (FQDN) to multiple IP addresses, Cribl Stream will sequentially use each address in the order they are returned by the DNS server for subsequent connection attempts.

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

## Proxying Requests

If you need to proxy HTTP/S requests, see [System Proxy Configuration](proxy-config).

## Troubleshooting {#troubleshooting}

[Snippet not found: content/shared/4.14/snippets/_destinations-troubleshooting.md]
