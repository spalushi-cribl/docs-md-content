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

### Authentication Settings {#auth}

SNMPv3 authentication provides secure access to devices through optional user authentication and decryption of incoming data packets. Cribl Stream lets you choose any of the following levels of security:

- User name checking without authentication.
- User authentication without privacy.
- User authentication with privacy.

**SNMPv3 authentication**: Toggle to `Yes` to enable SNMPv3 authentication and reveal authentication parameters. Defaults to `No`.

**Allow unmatched traps**: Toggle to `Yes` to pass through traps that don't match any of the configured users. Cribl Stream will not attempt to authenticate or decrypt these traps. When toggled to `No` (default), Cribl Stream drops traps without a correctly configured user name.

**v3 Name**: Enter the SNMPv3 user name (required). Multiple users are supported.

You must configure at least one user to enable SNMPv3 authentication. If desired, you can enter separate user credentials for each SNMPv3 user. The authentication protocol, authentication key, privacy protocol, and privacy key can be unique for each user.

For authentication to succeed, the user name and authentication protocol and key must match in the Source and the sending device configuration.

For decryption to succeed, the user name, authentication protocol and key, and the privacy protocol and key must match in the Source and the sending device configuration.

**Authentication protocol**: Select the authentication protocol required for your use case. If you select `None`, Cribl Stream will only check the user name without authenticating the user or decrypting the data. Otherwise, Cribl Stream uses the authentication protocol you specify and the key you provide to authenticate the user. 

Options include:
- `None`
- `MD5`
- `SHA1`
- `SHA224`
- `SHA256`
- `SHA384`
- `SHA512`

**v3 authentication key**: Enter the authentication key.

Choosing an authentication protocol also allows you to select an optional privacy protocol to decrypt incoming packets.

Cribl Stream logs authentication failures at the `debug` level.

**Privacy protocol**: Select the privacy protocol required for your use case. If you select `None`, Cribl Stream authenticates the user without decrypting the incoming data. Otherwise, Cribl Stream uses the privacy protocol you specify and the key you provide to decrypt the data. 

Options include:
- `None`
- `DES`
- `AES128`
- `AES256b (Blumenthal)`
- `AES2556r (Reeder)`

For every successfully decrypted trap, Cribl Stream adds `__didDecrypt: true` to the Event. Cribl Stream logs decryption failures at the `debug` level.

**v3 privacy key**: Enter the privacy key.

**Add user**: Click to add a new set of user credentials for SNMPv3 authentication.

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per PQ‑enabled Source, per Worker Process. 
> 
> The 1 GB limit is on uncompressed inbound data, and no compression is applied to the queue. This limit is not configurable. For configurable queue size, compression, mode, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Stream's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Stream's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. (This buffer is per connection, not just per Worker Process – and this can dramatically expand memory usage.)

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Stream applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume, on each Worker Process. Once this limit is reached, Cribl Stream will stop queueing data, and will block incoming events.  **This will cause UDP senders to drop events.** Enter a numeral with units of KB, MB, etc. If not specified, the implicit `0` default will enable Cribl Stream to fill all available disk space on the volume.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.

> In Cribl Stream 4.1 and later, Source-side PQ's default **Mode** is `Always on`, to best ensure events' delivery. For details on optimizing this selection, see [Always On versus Smart Mode](persistent-queues#source-pq-busy).
> 
> You can optimize Workers' startup connections and CPU load at **Group Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .info}

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**IP allowlist regex**: Regex matching IP addresses that are allowed to send data. Defaults to `.*`, i.e., all IPs.

**Max buffer size (events)**: Maximum number of events to buffer when downstream is blocking. Defaults to `1000`.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source's data to one or more Destinations via independent, direct connections.

## Internal Fields

Cribl Stream uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [Functions](functions) can use them to make processing decisions.

Fields for this Source:
 
* `__didDecrypt`: Set to `true` for every trap that is successfully decrypted.
* `__final`
* `__inputId`
* `__snmpRaw`: Buffer containing Raw SNMP packet
* `__snmpVersion`: Acceptable values are `0`, `2` , or `3`. These respectively indicate SNMP v1, v2c, and v3.
* `__srcIpPort` : In this particular Source, this field uses a pipe (`|`) symbol to separate the source IP address and the port, in this format: 
`event.__srcIpPort = ${rInfo.address}|${rInfo.port};`
* `_time`

## Considerations for Working with SNMP Trap Data

* It's possible to work with SNMP metadata (i.e., we'll decode the packet). Options include dropping, routing, etc.

* SNMP packets can be forwarded to other SNMP destinations. However, the contents of the incoming packet **cannot** be modified – i.e., we'll forward the packets verbatim as they came in. 

* SNMP packets can be forwarded to non-SNMP destinations (e.g., Splunk, Syslog, S3, etc.).

* Non-SNMP input data **cannot** be sent to SNMP destinations.
