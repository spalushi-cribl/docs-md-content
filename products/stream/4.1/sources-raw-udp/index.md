# UDP (Raw)



Cribl Stream supports receiving raw, unparsed data via UDP.

> Type: **Push**  |  TLS Support: **NO** | Event Breaker Support: **NO**
>
{.box .info}

## Configuring Cribl Stream to Receive Raw UDP Data

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **Raw UDP**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **Raw UDP**. Next, click **Add Source** to open a **New Source** modal that provides the options below.

> Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
> This results from restrictions of the UDP protocol.
> 
> To minimize the risk of data loss, deploy a hybrid Stream Worker Group
> with customer-managed Worker Nodes as close as possible to the UDP sender.
> Cribl also recommends tuning the OS UDP buffer size.
>
{.box .cloud}

### General Settings

**Input ID**: Enter a unique name to identify this raw UDP Source definition. 

**Address**: Enter the hostname/IP to listen for raw UDP data. For example: `localhost`, `0.0.0.0`, or `::`.

**Port**: Enter the port number.

### Optional Settings

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](deploy-cloud#enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per Worker Process.
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Stream's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Stream's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the Source. Defaults to `1000`.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Stream applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Stream will stop queueing data, and will apply the **Queue‑full behavior**. Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Stream to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> As of Cribl Stream 4.1, Source-side PQ's default **Mode** changed from `Smart` to `Always on`. This option more reliably ensures events' delivery, and the change does not affect existing Sources' configurations. However:
> - If you create Stream Sources programmatically, and you want to enforce the previous `Smart` mode, you'll need to update your existing code.
> - If you enable `Always on`, this can reduce data throughput. As a trade-off for data durability, you might need to either accept slower throughput, or provision more machines/faster disks.
> - You can optimize Workers' startup connections and CPU load at **Group Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .warning}

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 
  * **Name**: Field name.
  * **Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**Single msg per UDP**: Toggle to `Yes` if each UDP message should be treated as an independent event. Leave set to the default `No` if the message should be broken on newlines to create multiple events.

**Ingest raw bytes**: Toggle to `Yes` to add a `__rawBytes` field to each event containing an array of the bytes received as the UDP message.

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection. Defaults to `.*` (i.e, all IPs).

**Max buffer size (events)**: Maximum number of events to buffer when downstream is blocking. The buffer is only in memory.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## What Fields to Expect

The Raw UDP Source does minimal processing of the incoming UDP messages to remain consistent with the internal Cribl [event model](event-model). For each UDP message received on the socket, you can expect an event with the following fields:

  * `_raw`: Contains the UTF-8 representation of the entire message received (if **Single msg per UDP** is set to `Yes`), or of the given line that was split out of the message.
  * `_time`: The UNIX timestamp (in seconds) at which the message was received by Cribl Stream.
  * `source`: A string in the form `udp|<remote IP address>|<remote port>`, indicating the remote sender.
  * `host`: The hostname of the machine running Cribl Stream that ingested this event.

Also, the internal fields listed below will be present.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [functions](functions) can use them to make processing decisions.

Fields accessible for this Source:
  
  * `__inputId`
  * `__srcIpPort`
  * `__rawBytes`: When **Ingest raw bytes** is set to `Yes`, this field will be an array containing the bytes of the UDP message.
