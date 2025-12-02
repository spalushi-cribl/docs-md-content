# Google Cloud Logging Destination


Cribl Edge supports sending log data to [Google Cloud Logging](https://cloud.google.com/logging), a real-time log storage and management service with search, analysis and alerting.

> Type: Streaming | TLS Support: Yes | PQ Support: Yes 
>
{.box .info} 

## Configure Cribl Edge to Output to Google Cloud Logging

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**. 
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Google Cloud Logging definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Log location type**: Select an option from the dropdown to determine where your log entries are categorized. Choose **Project**, **organization**, **Billing Account**, or **Folder**. For details, see [Log Location Types](#types).
   - **Log name expression**: Enter a JavaScript expression to compute the log name's value.
   - **Validate and correct log name**: Toggle on to ensure that log names are sanitized and truncated for compliance and reliability. Log name expression characters that do not meet Google requirements are replaced by an underscore `_`. Additionally, the log name is truncated to a maximum of 512 characters, which is the upper limit enforced by Google Cloud Logging. Without sanitization, Google Cloud Logging will reject requests with invalid log names, resulting in dropped events and potential data loss.
3. Under **Field Mappings**, configure the following: 
     - **Payload format**: Select an option from the dropdown specify how data is structured within a log entry. Choose `Text` or `JSON`. For details, see [Field Mappings](#map)
        > This Destination compresses all payloads, using gzip.
        {.box .success}
     - **Log labels**: Click **Add label** to apply one or more labels to log entries. On each row of the resulting table, define each label with:
     - **Label**: Arbitrary name for the label.
     - **Value**: JavaScript expression to compute the label's value.
     - **Resource type expression**: JavaScript expression to compute the field value for the managed resource type. Must evaluate to one of the [monitored resource types listed here](https://cloud.google.com/logging/docs/api/v2/resource-list#resource-types). If not specified, falls back to `global`.
     - **Resource labels**: Click **Add label** to apply one or more labels to the managed resource. For details, see [Resource Labels](#labels).
    - **Severity expression**: JavaScript expression to compute the severity field's value. Must evaluate to one of the `LogSeverity` values [listed here](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry#logseverity). Case-insensitive. If not specified, falls back to `DEFAULT`.
    - **Insert ID expression**: JavaScript expression to compute the insert ID field's value.
4. Next, you can configure the following **Optional Settings**:
   - **Backpressure behavior**: Whether to block, drop, or queue events when all receivers are exerting backpressure. Defaults to `Block`.   
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. To add **Indexed Fields** to your [LogEntry objects](https://cloud.google.com/logging/docs/reference/v2/rest/v2/LogEntry), provide JavaScript expressions to populate fields beyond the standard LogEntry structure, enabling richer analysis and correlation within Chronicle. For each field you want to index, provide a JavaScript expression. Ensure the expression's result matches the expected data type defined in the LogEntry documentation. Mismatched types prevent the field from appearing in Chronicle.
   * You can configure the following indexed fields using JavaScript expressions: **Trace expression**, **Span ID expression**, **Trace sampled expression**, **HTTP Request**, **Operation**, **Source Location**, and **Log Split**.
   > The LogEntry object's `timestamp` field is populated from the event's `_time` field.
   {.box .info}
6. Optionally, you can adjust the [Authentication](#auth), [Processing](#processing), and [Advanced](#advanced-tab) settings outlined in the sections below.
7. Select **Save**, then **Commit & Deploy**. 


### Log Location Types {#types}
Log Location Types allow you to categorize your log entries based on organizational structures. Choose one of the following options to associate log entries with a specific project, organization, billing account, or folder:

 - `Project`: Displays a **Project ID expression** field. Here, enter a JavaScript expression to compute the value of the project ID to associate log entries with.
 - `Organization`: Displays a **Organization ID expression** field. Enter a JS expression to compute the value of the organization ID to associate log entries with.
- `Billing Account`: Displays a **Billing account ID expression** field. Enter a JS expression to compute the value of the billing account ID to associate log entries with.
 - `Folder`: Displays a **Folder expression** field. Enter a JS expression to compute the value of the folder ID to associate log entries with.

### Field Mappings
Field Mappings specify how log entries should be structured. Choose one of the following options:

 - `Text`: This default option displays a **Payload text expression** field. Here, enter JavaScript expression to compute the payload's value. Must evaluate to a JavaScript string value; if validation fails, this will default to a JSON string representation of the event.

 - `JSON`: Displays a **Payload object expression** field. Enter a JS expression to compute the payload's value. Must evaluate to a JavaScript object value; if validation fails, this will default to the entire event.

### Resource Labels {#labels}
Each **Resource label** must correspond to a [valid label](https://cloud.google.com/logging/docs/api/v2/resource-list#resource-types) for the resource type you've specified in the **Resource type expression**, or else Google Cloud Logging will drop it. On each row of the resulting table, define each label with:
- **Label**: Arbitrary name for the label.
- **Value**: JavaScript expression to compute the label's value.

### Authentication {#auth}

Use the **Authentication method** drop-down to select one of these options:

- **Auto**: This option uses the `GOOGLE_APPLICATION_CREDENTIALS` environment variable, and requires no configuration here.

- **Manual**: This default option displays a **Service account credentials** field for you to enter the contents of your service account credentials file (a set of JSON keys), as downloaded from Google Cloud. To insert the file itself, click the upload button at this field's upper right. As an alternative, you can use environment variables, as outlined [here](https://cloud.google.com/docs/authentication/provide-credentials-adc).

- **Secret**: This option exposes a drop-down in which you can select a [stored secret](/stream/securing-secrets) that references the service account credentials described above. A **Create** link is available to store a new, reusable secret.


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

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Advanced Settings {#advanced-tab}

**Request concurrency**: Maximum number of concurrent requests before blocking. This is set per Worker Process. Defaults to `5`.

**Connection timeout**: Amount of time (in milliseconds) to wait for the connection to establish before retrying. Defaults to `10000` (10 sec.).

**Request timeout**: Amount of time (in seconds) to wait for a request to complete before aborting it. Defaults to `30` sec.

**Body size limit (KB)**: Maximum size of the request body before compression. Defaults to `4096` KB. The actual request body size might exceed the specified value because the Destination adds bytes when it writes to the downstream receiver. Cribl recommends that you experiment with the **Body size limit** value until downstream receivers reliably accept all events.

**Buffer memory limit (KB)**: Total amount of memory used to buffer outgoing requests waiting to be sent. If left blank, defaults to 5 times the max body size (if set). If 0, no limit is enforced. This provides granular control over the memory allocated for buffering outgoing batched requests. Increasing the limit allows batches to grow larger before being flushed, improving efficiency for data with high cardinality (a large number of unique batches). Finding the optimal balance between efficient data transfer and memory usage involves adjusting both the **Buffer memory limit** and **Body size limit** settings.

**Events-per-request limit**: Maximum number of events to include in the request body. Defaults to `0` (unlimited).

**Flush period (sec)**: Maximum time between requests. Low values could cause the payload size to be smaller than the configured **Max body size**. Defaults to `1` sec.

**Throttle request rate**: Enter a maximum number of requests to send per second, as necessary to comply with Google Cloud's API limits. This field defaults to empty (no throttling), and accepts throttling rates as high as `2000` requests per second.

**Environment**: Optionally, specify a single Git branch on which to enable this configuration. If this field is empty, the config will be enabled everywhere.

## Google Cloud Roles and Permissions

Your Google Cloud service account must have at least the following role:
- `Logs Writer`

## Troubleshooting

The Destination's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](managing-destinations#capture-outgoing-data) to see real-time events as they flow through the Destination. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the delivery process, including any errors or warnings that may have occurred.

**Test**: Ensures that the Destination is correctly set up and reachable. Verify that sample events are sent correctly by clicking **Run Test**.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify delivery issues. Analyze the graphs showing events and bytes in/out over time.