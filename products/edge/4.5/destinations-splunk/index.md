# Splunk Single Instance


This Splunk Destination can stream data to a **free** Splunk Cloud instance. From the perspective of the receiving Splunk Cloud instance, the data arrives cooked and parsed.

For a **Standard** Splunk Cloud instance whose `../default/outputs.conf` file contains multiple indexer entries, you must instead use Cribl Edge's [Splunk Load Balanced](destinations-splunk-lb) Destination.

> Type: Streaming | TLS Support: Configurable | PQ Support: Yes 
>
{.box .info} 

## Configuring Cribl Edge to Output to Splunk Destinations

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Destination** at right. From the resulting drawer's tiles or the **Destinations** left nav, select **Splunk** > **Single Instance**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Destinations** (Stream) or **More** > **Destinations** (Edge). From the resulting page's tiles, select **Splunk** > **Single Instance**. Next, click **Add Destination** to open a **New Destination** modal that provides the options below.

### General Settings

**Output ID**: Enter a unique name to identify this Splunk Single Instance definition.

**Address**: Hostname of the Splunk receiver.

**Port**:  The port number on the host.

### Optional Settings

**Backpressure behavior**: Select whether to block, drop, or queue events when all receivers are exerting backpressure. (Causes might include a broken or denied connection, or a rate limiter.) Defaults to `Block`.

**Tags**: Optionally, add tags that you can use to filter and group Destinations in Cribl Edge's **Manage Destinations** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Persistent Queue Settings

> This tab is displayed when the **Backpressure behavior** is set to **Persistent Queue**. 
>
{.box .info}

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the destructive **Clear Persistent Queue** button (described below in this section). A maximum queue size of 1 GB disk space is automatically allocated per PQ‑enabled Destination, per Worker Process. The 1 GB limit is on outbound uncompressed data, and no compression is applied to the queue. 
> 
> This limit is not configurable. If the queue fills up, Cribl Edge will block outbound data. To configure the queue size, compression, queue-full fallback behavior, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Max file size**: The maximum data volume to store in each queue file before closing it. Enter a numeral with units of KB, MB, etc. Defaults to `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Destination will stop queueing data and apply the **Queue‑full** behavior. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, etc. Can be set as high as `1 TB`, unless you've [configured](settings-group-fleet#storage) a different **Max PQ size per Worker Process** in Fleet Settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this value, Cribl Edge will append `/<worker‑id>/<output‑id>`.

**Compression**: Codec to use to compress the persisted data, once a file is closed. Defaults to `None`; `Gzip` is also available.

**Queue-full behavior**: Whether to block or drop events when the queue is exerting backpressure (because disk is low or at full capacity). **Block** is the same behavior as non-PQ blocking, corresponding to the **Block** option on the **Backpressure behavior** drop-down. **Drop new data** drops the newest events from being sent out of Cribl Edge, and throws away incoming data, while leaving the contents of the PQ unchanged.

**Clear Persistent Queue**: Option to delete the files that are currently queued for delivery to this Destination. Select **Clear Persistent Queue** and accept the confirmation to free up disk space by permanently deleting the queued data without delivering it to downstream receivers. (Appears only after **Output ID** has been defined.)

**Strict ordering**: The default `Yes` position enables FIFO (first in, first out) event forwarding. When receivers recover, Cribl Edge will send earlier queued events before forwarding newly arrived events. To instead prioritize new events before draining the queue, toggle this off. Doing so will expose this additional control:
- **Drain rate limit (EPS)**: Optionally, set a throttling rate (in events per second) on writing from the queue to receivers. (The default `0` value disables throttling.) Throttling the queue's drain rate can boost the throughput of new/active connections, by reserving more resources for them. You can further optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).

### TLS Settings (Client Side)

**Enabled** defaults to `No`. When toggled to `Yes`:

**Validate server certs**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (e.g., the system's CA). Defaults to `Yes`.

**Server name (SNI)**: Server name for the SNI (Server Name Indication) TLS extension. This must be a host name, not an IP address.

**Minimum TLS version**: Optionally, select the minimum TLS version to use when connecting.

**Maximum TLS version**: Optionally, select the maximum TLS version to use when connecting.

**Certificate name**: The name of the predefined certificate.
 
**CA certificate path**: Path on client containing CA certificates (in PEM format) to use to verify the server's cert. Path can reference `$ENV_VARS`.
 
**Private key path (mutual auth)**: Path on client containing the private key (in PEM format) to use.  Path can reference `$ENV_VARS`. **Use only if mutual auth is required**.
 
**Certificate path (mutual auth)**: Path on client containing certificates in (PEM format) to use. Path can reference `$ENV_VARS`. **Use only if mutual auth is required**. 
 
**Passphrase**: Passphrase to use to decrypt private key.

> ##### Single .pem File
>
> If you have a **single** .pem file containing `cacert`, `key`, and `cert` sections, enter it in all of these fields above: **CA certificate path**, **Private key path (mutual auth)**, and **Certificate path (mutual auth)**.
>
{.box .info}

### Timeout Settings

**Connection timeout**: Amount of time (in milliseconds) to wait for the connection to establish, before retrying. Defaults to `10000`.

**Write timeout**: Amount of time (in milliseconds) to wait for a write to complete, before assuming connection is dead. Defaults to `60000`.

### Processing Settings

#### Post‑Processing

**Pipeline**: Pipeline to process data before sending the data out using this output.

**System fields**: A list of fields to automatically add to events that use this output. By default, includes `cribl_pipe` (identifying the Cribl Edge Pipeline that processed the event). Supports wildcards. Other options include:

- `cribl_host` – Cribl Edge Node that processed the event.
- `cribl_input` – Cribl Edge Source that processed the event.
- `cribl_output` – Cribl Edge Destination that processed the event.
- `cribl_route` – Cribl Edge Route (or QuickConnect) that processed the event.
- `cribl_wp` – Cribl Edge Worker Process that processed the event.

### Advanced Settings

**Output multiple metrics**: Toggle to `Yes` to output multiple-measurement metric data points. (Supported in Splunk 8.0 and above, this format enables sending multiple metrics in a single event, improving the efficiency of your Splunk capacity.)

**Minimize in-flight data loss**: If set to `Yes` (the default), Cribl Edge will check whether the indexer is shutting down, and if so, will stop sending data. This helps minimize data loss during shutdown. (Note that Splunk logs will indicate that the Cribl app has set `UseAck` to `true`, even though Cribl does not enable full `UseAck` behavior.) If toggled to `No`, exposes the following alternative option: {{< id `ack` >}}

**Max failed health checks**: Displayed (and set to `1` by default) only if **Minimize in‑flight data loss** is disabled. This option sends periodic requests to Splunk once per minute, to verify that the Splunk endpoint is still alive and can receive data. Its value governs how many failed requests Cribl Edge will allow before closing this connection. 

> A low threshold value improves connections' resilience, but by proliferating connections, this can complicate troubleshooting. Set to `0` to disable health checks entirely – here, if the connection to Splunk is forcibly closed, you risk some data loss.
>
{.box .warning}



**Max S2S version**: The highest version of the Splunk-to-Splunk protocol to expose during handshake. Defaults to `v3`; `v4` is also available.

**Throttling**: Throttle rate, in bytes per second. Defaults to `0`, meaning no throttling. Multiple-byte units such as KB, MB, GB etc. are also allowed, e.g., `42 MB`. When throttling is engaged, your **Backpressure behavior** selection determines whether Cribl Edge will handle excess data by blocking it, dropping it, or queueing it to disk.

**Nested field serialization**: Specifies how to serialize nested fields into index-time fields. Defaults to `None`.

**Authentication method**: Use the buttons to select one of these options:

- **Manual**: In the resulting **Auth token** field, enter the shared secret token to use when establishing a connection to a Splunk indexer.

- **Secret**: This option exposes an **Auth token (text secret)** drop-down, in which you can select a [stored secret](/stream/securing-and-monitoring#secrets) that references the auth token described above. A **Create** link is available to store a new, reusable secret.

**Log failed requests to disk**: Toggling to `Yes` makes the payload of the most recently failed request available for inspection. See [Inspect Payload to Troubleshoot Closed Connections](#log-payload) below.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### Inspect Payload to Troubleshoot Closed Connections {#log-payload}

When a downstream receiver closes connections from this Destination (or just stops responding), inspecting the payload of the most recently failed request can help you find the cause. For example:

* Suppose you send an event whose size is larger than the downstream receiver can handle. 
* Suppose you send an event that has a `number` field, but the value exceeds the highest number that the downstream receiver can handle.

When **Log failed requests to disk** is enabled, you can inspect the last failed request payload. Here is how:

1. In the Destination UI, navigate to the **Logs** tab.
2. Find a log entry with a `connection error` message. 
3. Expand the log entry. 
4. If the message includes the phrase `See payload file for more info`, note the path in the `file` field on the next line.

Now you have the path to the directory where Cribl Edge is storing the payload from the last failed request.

## Notes about Forwarding to Splunk 

* Data sent to Splunk is not compressed.

- The only `ack` from indexers that Cribl Edge listens for and acts upon is the shutdown signal described in [Minimize in-flight data loss](#ack) above. 

* If events have a Cribl Edge internal field called `__criblMetrics`, they'll be forwarded to Splunk as metric events. 

* If events do **not** have a `_raw` field, they'll be serialized to JSON prior to sending to Splunk.

* See [Splunk's documentation](https://docs.splunk.com/Documentation/SplunkCloud/latest/Data/Configureindex-timefieldextraction#Add_an_entry_to_fields.conf_for_the_new_field) on editing `fields.conf` to ensure the visibility of index-time fields sent to Splunk by Cribl Edge.

## Troubleshooting Resources {#troubleshooting}

[Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Destination Integrations: Splunk Cloud](https://university.cribl.io/#/online-courses/7d6db44d-622f-4af4-8edb-429be2ebcc28) short course. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
