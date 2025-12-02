# SNMP Trap


Cribl Stream supports receiving data from SNMP Traps.

> Type: **Push**  |  TLS Support: **NO** | Event Breaker Support: **No**
>
{.box .info}

## Configuring Cribl Stream to Receive SNMP Traps

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **SNMP Trap**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **SNMP Trap**.  Next, click **New Source** to open a **New Source** modal that provides the options below.

> Cribl Stream, except in Cribl.Cloud, ships with an SNMP Trap Source preconfigured to listen on port `9162`. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

### General Settings

**Input ID**: Enter a unique name to identify this Source definition. 

**Address**: Address to bind on. Defaults to `0.0.0.0` (all addresses).

**UDP Port**: Port on which to receive SNMP traps. Defaults to `162`.

> Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
> This results from restrictions of the UDP protocol.
> 
> To minimize the risk of data loss, deploy a hybrid Stream Worker Group
> with customer-managed Worker Nodes as close as possible to the UDP senders.
> Cribl also recommends tuning the OS UDP buffer size.
>
{.box .cloud}

### Optional Settings

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Stream's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

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

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**IP allowlist regex**: Regex matching IP addresses that are allowed to send data. Defaults to `.*`, i.e., all IPs.

**Max buffer size (events) **: Maximum number of events to buffer when downstream is blocking. Defaults to `1000`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:
 
  * `__inputId`
  * `__srcIpPort` : In this particular Source, this field uses a pipe (`|`) symbol to separate the source IP address and the port, in this format: 
`event.__srcIpPort = ${rInfo.address}|${rInfo.port};`
  * `__snmpVersion`: Acceptable values are `0`, `2` , or `3`. These respectively indicate SNMP v1, v2c, and v3.
  * `__snmpRaw`: Buffer containing Raw SNMP packet

## Considerations for Working with SNMP Trap Data

* It's possible to work with SNMP metadata (i.e., we'll decode the packet). Options include dropping, routing, etc.

* SNMP packets can be forwarded to other SNMP destinations. However, the contents of the incoming packet **cannot** be modified – i.e., we'll forward the packets verbatim as they came in. 

* SNMP packets can be forwarded to non-SNMP destinations (e.g., Splunk, Syslog, S3, etc.).

* Non-SNMP input data **cannot** be sent to SNMP destinations.
