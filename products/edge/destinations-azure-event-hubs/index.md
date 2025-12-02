# Azure Event Hubs Destination


Cribl Edge supports sending data to [Azure Event Hubs](https://azure.microsoft.com/en-us/services/event-hubs/).

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes
>
> Azure Event Hubs uses a binary protocol over TCP. It does not support HTTP proxies, so Cribl Edge must send events directly to receivers. You might need to adjust your firewall rules to allow this traffic.
>
> For configuration examples, see [Resources](#resources) below.
>
{.box .info}

See Microsoft's [Compare Azure Event Hubs Tiers](https://learn.microsoft.com/en-us/azure/event-hubs/compare-tiers) topic to configure tiers whose features and quotas match your desired data volume and throughput.

## Configuring Cribl Edge to Output to Azure Event Hubs

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
   - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Destination** and select the Destination you want from the list, choosing either **Select Existing** or **Add New**.
   - To configure via the [Routes](routes), select **Data** > **Destinations** or **More** > **Destinations** (Edge). Select the Destination you want. Next, select **Add Destination**.
2. In the **New Destination** modal, configure the following under **General Settings**:
   - **Output ID**: Enter a unique name to identify this Azure Event Hubs definition. If you clone this Destination, Cribl Edge will add `-CLONE` to the original **Output ID**.
   - **Description**: Optionally, enter a description.
   - **Brokers**: List of Event Hub Kafka brokers to connect to. (For example, `yourdomain.servicebus.windows.net:9093`.) Find the hostname in Shared Access Policies, in the host portion of the primary or secondary connection string. If omitted, the port will default to `9093`.
   - **Event Hub name**:  The name of the Event Hub (a.k.a., Kafka Topic) on which to publish events. Can be overwritten using the `__topicOut` field.
3. Next, you can configure the following **Optional Settings**:
   - **Acknowledgments**:  Control the number of required acknowledgments. Defaults to `Leader`. Setting this control to `Leader` or `All` may significantly slow throughput. If the impact is too great for your use case, set this control to `None`.
   - **Record data format**: Format to use to serialize events before writing to the Event Hub Kafka brokers. Defaults to `JSON`.
     - `JSON` serializes the entire event from your Pipeline, including all metadata and enriched fields, into a single JSON object.
     - `Field _raw` sends only the contents of the `_raw` field as the payload. This option provides maximum throughput and minimal overhead.
   - **Backpressure behavior**: Whether to block, drop, or queue events when all receivers are exerting backpressure. Defaults to `Block`.
   - **Tags**: Optionally, add tags that you can use to filter and group Destinations on the **Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [TLS](#tls), [Authentication](#auth), [Persistent Queue](#pq), [Processing](#processing),  [Parquet](#parquet-settings), [Retries](#retries) and [Advanced](#advanced-tab) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### TLS Settings (Client Side) {#tls}

 **Enabled**: Defaults to toggled on.

 **Validate server certs**: Defaults to toggled on.

### Authentication {#auth}

Authentication parameters to use when connecting to brokers. Using [TLS](#tls_settings) is highly recommended.

**Enabled**: If toggled on (default), this section's remaining settings are displayed, and all are required settings.

**SASL mechanism**: SASL (Simple Authentication and Security Layer) authentication mechanism to use with Kafka brokers. Defaults to `PLAIN`, which exposes [Basic Authentication](#plain) options that rely on Azure Event Hubs connection strings. Select `OAUTHBEARER` to enable [OAuth Authentication](#oauthbearer) via a different set of options.

#### Basic Authentication {#plain}

Selecting the `PLAIN` SASL mechanism provides the options listed in this section.

**Username**: The username for authentication. For Event Hubs, this should always be `$ConnectionString`.

**Authentication method**: Use the buttons to select one of these options:
- **Manual**: Use this default option to enter your Event Hubs connection string's primary or secondary key from the Event Hubs workspace. Exposes a **Password** field for this purpose.
- **Secret**: This option exposes a **Password (text secret)** drop-down, in which you can select a stored secret that references an Event Hubs connection string. The secret can reside in Cribl Edge's [internal secrets manager](securing-kms-config) or (if enabled) in an external KMS. A **Create** link is available if you need a new secret.

##### Connection String Format

Either authentication method above uses an Azure Event Hubs connection string in this format:

`Endpoint=sb://<FQDN>/;SharedAccessKeyName=<your‑shared-access‑key-name>;SharedAccessKey=<your‑shared-access‑key-value>`

A fictitious example is:

`Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

#### OAuth Authentication {#oauthbearer}

Selecting the `OAUTHBEARER` SASL mechanism provides the options listed in this section.

**Microsoft Entra ID authentication endpoint**: Specifies the Microsoft Entra ID endpoint from which to acquire authentication tokens. Defaults to `https://login.microsoftonline.com`. You can instead select `https://login.microsoftonline.us` or `https://login.partner.microsoftonline.cn`.

**Client ID**: Enter the `client_id` to pass in the OAuth request parameter.

**Tenant identifier**: Enter your Microsoft Entra ID subscription's directory ID (tenant ID).

**Scope**: Enter the scope to pass in the OAuth request parameter. This will be of the form: `https://<Event‑Hubs‑Namespace-Host-name>/.default`. (For example, for an Event Hubs Namespace > Host name: `goatyoga.servicebus.windows.net`, **Scope**: `https://goatyoga.servicebus.windows.net/.default`.)

**Authentication method**: Use the buttons to select one of these options:
- **Manual**: This default option exposes a **Client secret** field, in which to directly enter the `client_secret` to pass in the OAuth request parameter.
- **Secret**: Exposes a **Client secret (text secret)** drop-down, in which you can select a stored [secret](securing-secrets) that references the `client_secret`. A **Create** link is available to define a new secret.
- **Certificate**: Exposes a **Certificate name** drop-down, in which you can select a stored [certificate](securing-ssl). A **Create** link is available to define a new cert.


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

### Retries {#retries}

These settings provide flexibility in handling retries for failed messages, allowing you to balance between quick retries and avoiding excessive load on the system.

The default configuration starts with a 300 milliseconds retry interval, doubles the interval after each retry, and caps the maximum retry interval at 30 seconds. The system will attempt to retry the request up to five times before considering it a failure.

**Retry limit**: Maximum number of times to retry a failed request before the message fails. Defaults to `5`. Enter `0` to not retry at all. For example, if set to `5`, the system will attempt to retry the request up to five times before considering it a failure.

**Initial retry interval (ms)**: Initial value used to calculate the retry interval, in milliseconds. This value determines the starting point for the retry delay. The default (and minimum) is `300` ms (three seconds). The maximum allowed value is `600000` ms (10 minutes). For example, if set to `1000` ms, the first retry will occur after one second.

**Backoff multiplier**: Set the backoff multiplier (`2-20`) to control the retry frequency for failed messages. The multiplier is used in an exponential backoff formula to increase the delay between retries. For faster retries, use a lower multiplier. For slower retries with more delay between attempts, use a higher multiplier. For example, with an initial retry interval of `1000` ms and a multiplier of `2`, the retry intervals will be 1,000 ms, 2,000 ms, 4,000 ms, and so on. See the Kafka [documentation](https://kafka.js.org/docs/retry-detailed) for details.

**Backoff limit (ms)**: The maximum wait time for a retry, in milliseconds. This setting caps the exponential backoff delay to prevent excessively long wait times. The default (and minimum) value is `30000` ms (30 seconds), and the maximum is `180000` ms (180 seconds). For example, if the calculated retry interval exceeds 180,000 ms, the retry will occur after 180,000 ms instead.

### Advanced Settings {#advanced-tab}

**Record size limit (KB, uncompressed)**: Maximum size (KB) of each record batch before compression. Setting should be < `message.max.bytes` settings in Kafka brokers. Defaults to `768`.

**Events-per-batch limit**: Maximum number of events in a batch before forcing a flush. Defaults to `1000`.

**Flush period (sec)**: Maximum time between requests. Low settings could cause the payload size to be smaller than its configured maximum. Defaults to `1`.

**Connection timeout (ms)**: Maximum time to wait for a successful connection. Defaults to `10000` ms (10 seconds). Valid range is `1000` to `3600000` ms (1 second to 1 hour).

**Request timeout (ms)**: Maximum time to wait for a successful request. Defaults to `60000` ms (1 minute).

**Authentication timeout (ms)**: Maximum time to wait for Kafka to respond to an authentication request. Defaults to `1000` (1 second).

**Reauthentication threshold (ms)**: If the broker requires periodic reauthentication, this setting defines how long before the reauthentication timeout Cribl Edge initiates the reauthentication. Defaults to `10000` (10 seconds).

A small value for this setting, combined with high network latency, might prevent the Destination from reauthenticating before the Kafka broker closes the connection.

A large value might cause the Destination to send reauthentication messages too soon, wasting bandwidth.

The Kafka setting `connections.max.reauth.ms` controls the reuthentication threshold on the Kafka side.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

## Working with Event Timestamps {#timestamps}

By default, when an incoming event reaches this Destination, the underlying Kafka JS library adds a `timestamp` field set to the current time at that moment. The library gets the current time from the `Date.now()` JavaScript method. Events that this Destination sends to downstream Kafka receivers include this `timestamp` field.

Some production situations require the `timestamp` value to be the time an incoming event was originally created by an upstream sender, not the current time of its arrival at this Destination. For example, suppose the events were originally created over a wide time range, and arrive at the Destination hours or days later. In this case, the original creation time might be important to retain in events Cribl Edge sends to downstream Kafka receivers.

Here is how to satisfy such a requirement:

1. In a Pipeline that routes events to this Destination, use an Eval Function to add a field named `__kafkaTime` and write the incoming event's original creation time into that field. The value of `__kafkaTime` **must** be in UNIX epoch time ([either seconds or milliseconds since the UNIX epoch](https://developer.mozilla.org/en-US/docs/Glossary/Unix_time) – both will work). For context, see [this explanation](/stream/sources-msk#how-criblstream-handles-the-_time-field) of the related fields `_time` and `__origTime`.
2. This Destination will recognize the `__kafkaTime` field and write its value into the `timestamp` field. (This is similar to the way you can use the `__topicOut` field to overwrite a topic setting, as described [above](#general-settings).)

If the `__kafkaTime` field is **not** present, the Destination will apply the default behavior (using `Date.now()`) described previously.

## Azure Permissions

When configuring an Azure Destination in Cribl Edge, you must grant the necessary permissions for it to interact with your Azure environment. Cribl recommends creating an Azure service principal with the minimum required permissions using Azure role-based access control (Azure RBAC). This approach ensures Cribl Edge has a dedicated identity with precisely the access it needs, following the principle of least privilege.

The minimum permissions required for Destinations are:

- **Azure Event Hubs Data Sender**

See [Authorize access to Azure Event Hubs resources using Microsoft Entra ID](https://learn.microsoft.com/en-us/azure/event-hubs/authorize-access-azure-active-directory) in the official Microsoft documentation for more information.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in forwarding data to a Destination.

Fields for this Destination:

  * `__topicOut`
  * `__key`
  * `__headers`
  * `__keySchemaIdOut`
  * `__valueSchemaIdOut`

## Resources {#resources}

For examples of configuring Cribl Edge to interoperate with Azure services, see these guides:

- [Preparing the Azure Workspace](/stream/usecase-azure-workspace/).

- [SSO with Microsoft Entra ID (Cribl.Cloud)](sso-microsoft-entra-id-cloud).

- [SSO with Microsoft Entra ID (On-Prem)](sso-microsoft-entra-id-on-prem).

## Troubleshooting {#troubleshooting}

The Destination's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](managing-destinations#capture-outgoing-data) to see real-time events as they flow through the Destination. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the delivery process, including any errors or warnings that may have occurred.

**Test**: Ensures that the Destination is correctly set up and reachable. Verify that sample events are sent correctly by clicking **Run Test**.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify delivery issues. Analyze the graphs showing events and bytes in/out over time.

> [Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Destination Integrations: Azure Event Hubs](https://university.cribl.io/#/online-courses/e6b91967-87fa-4771-a40c-c86778b61a47) short course. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
>
{.box .course}