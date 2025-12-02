# Syslog Source


The Syslog Source receives syslog data (UDP/TCP) from various devices. Syslog messages are parsed into structured fields or stored in a raw format if unrecognized. For high-volume scenarios, TCP load balancing distributes data, optimizing performance and minimizing CPU strain.

Compatible RFCs:
- Supports structured syslog data defined by [RFC 3164](https://tools.ietf.org/html/rfc3164) and [RFC 5424](https://tools.ietf.org/html/rfc5424), which can include timestamps, facility codes, severities, and message content for detailed analysis.
- Processes message-length prefixes specified in [RFC 5425](https://datatracker.ietf.org/doc/html/rfc5425#section-4.3.1) and [RFC 6587](https://datatracker.ietf.org/doc/html/rfc6587#section-3.4.1), ensuring complete and accurate data transmission.

> Type: **Push**  |  TLS Support: **YES** | Event Breaker Support: **No**
>
> For details on how to replace your syslog server with Cribl Edge, and for configuration examples, see these resources:
> - [Syslog Best Practices](/stream/usecase-syslog/).
> - [Syslog to Cribl Stream Reference Architecture](/reference-architectures/reference-arch-syslog).
> - [Palo Alto Syslog to Cribl.Cloud](/stream/usecase-syslog-cloud/).
> - [CEF/LEEF Syslog Pack](https://packs.cribl.io/packs/cribl-cef-pack).
> - [Exabeam Pack for Palo Alto Networks NGFW](https://packs.cribl.io/packs/Exabeam_Pack_for_Palo_Alto).
>
{.box .info}

## Configure Cribl Edge to Receive Data over Syslog

> Cribl Edge ships with a Syslog Source preconfigured to listen for both UDP and TCP traffic on Port 9514. You can clone or directly modify this Source to further configure it, and then enable it.
>
{.box .success}

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this Syslog Source definition. The default Source is prefilled with the value `in_syslog_default`. which can't be changed via the UI. If you clone this Source, Cribl Edge will add `-CLONE` to the original **Input ID**.
    > The default filter expression from the **Input ID** field, `__inputId.startsWith('syslog:in_syslog:')` includes a trailing colon, which is omitted in the **Preview Full** mode of [Data Preview](data-preview). Therefore, when testing Routes and Pipelines, adjust your filter by removing the colon to match **Preview Full**. This issue is limited to testing and does not affect normal data processing.
    {.box .warning}
    - **Description**: Optionally, enter a description.
    - **Address**: Enter the hostname/IP on which to listen for data. For example, `localhost` or `0.0.0.0`. For details, see [IP Address Considerations](#address).
    - **UDP port**: Enter the UDP port number to listen on. Not required if listening on TCP. The maximum supported inbound UDP message size is 16,384 bytes. For details, see [UDP Port Considerations](#consider).
    - **TCP port**: Enter the TCP port number to listen on. Not required if listening on UDP.
3. Next, you can configure the following **Optional Settings**:
    - **Fields to keep**: List of fields from source data to retain and pass through. Supports wildcards. Defaults to `*` wildcard, meaning keep all fields. Fields **not** specified here (by wildcard or specific name) will be removed from the event.
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in the Cribl Edge UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [TLS](#tls), [Persistent Queue](#pq), [Processing](#processing) and [Advanced](#advanced-settings) settings, or [Connected Destinations](#connected) outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### IP Address Considerations {#address}

Cribl recommends that you change the **Address** setting from its default `0.0.0.0` only if you know precisely why you're doing so. There are very few use cases where changing this broad default  makes sense. All your Worker Nodes in each Worker Group receive this config, and no two Nodes should have the same IPs, other than `0.0.0.0` or `127.0.0.1`. The `0.0.0.0`  tells the system to listen to all interfaces and addresses. A `127.0.0.1` entry means listen **only** on `localhost`.

### UDP Port Considerations {#consider}

Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data. This is due to the UDP protocol's restrictions. To minimize the risk of data loss, deploy a customer-managed (hybrid) Stream Worker Group with Worker Nodes as close as possible to the UDP senders. Cribl also recommends tuning the OS UDP buffer size.

> ##### Listening on Port 514 {#port-514}
>
> For Syslog devices that are hardcoded to send to port 514, you can listen on this port on Cribl.Cloud, which otherwise is normally available only with privileged access.
> To do this, enable the built-in `in_syslog_default` Syslog Source, which forwards traffic from port 514 to 10514.
>
> Note that traffic on port 514 is unencrypted.
> This means that using this port may result in collecting fake or undesirable data.
> Enable the `in_syslog_default` Source only if you are aware of this risk.
>
{.box .cloud}

### TLS Settings (TCP Only) {#tls}

**Enabled**: Defaults to toggled off. When toggled on:

**Certificate**: Name of the predefined certificate.

**Private key path**: Server path containing the private key (in PEM format) to use. Path can reference `$ENV_VARS`.

**Passphrase**: Passphrase to use to decrypt private key.

**Certificate path**: Server path containing certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**CA certificate path**: Server path containing CA certificates (in PEM format) to use. Path can reference `$ENV_VARS`.

**Authenticate client (mutual auth)**: Require clients to present their certificates. Used to perform mutual authentication using SSL certs. Default is toggled off. When toggled on:

- **Validate client certificates**: Reject certificates that are not authorized by a CA in the **CA certificate path**, or by another trusted CA (for example, the system's CA). Default is toggled on.

- **Common name**: Regex that a peer certificate's subject attribute must match in order to connect. Defaults to `.*`. Matches on the substring after `CN=`. As needed, escape regex tokens to match literal characters. (For example, to match the subject `CN=worker.cribl.local`, you would enter: `worker\.cribl\.local`.) If the subject attribute contains Subject Alternative Name (SAN) entries, the Source will check the regex against all of those but ignore the Common Name (CN) entry (if any). If the certificate has no SAN extension, the Source will check the regex against the single name in the CN.

**Minimum TLS version**: Optionally, select the minimum TLS version to accept from connections.

**Maximum TLS version**: Optionally, select the maximum TLS version to accept from connections.

### Persistent Queue Settings {#pq}

In the **Persistent Queue Settings** tab, you can optionally specify persistent queue storage, using the following controls. Persistent queue buffers and preserves incoming events when a downstream Destination has an outage or experiences backpressure.

Before enabling persistent queue, learn more about persistent queue behavior and how to optimize it with your system:

- [About Persistent Queues](/persistent-queues)
- [Optimize Source Persistent Queues (sPQ)](/persistent-queues-sources)

> On Cribl-managed [Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the **Enable persistent queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per PQ‑enabled Source, per Worker Process.
>
> The 1 GB limit is on uncompressed inbound data, and the queue does not perform any compression. This limit is not configurable. For configurable queue size, compression, mode, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
{.box .cloud}

**Enable persistent queue**: Default is toggled off. When toggled on:

**Mode**: Select a condition for engaging persistent queues.

- `Always On`: This default option will always write events to the persistent queue, before forwarding them to the Cribl Stream data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from the Cribl Stream data processing engine.

> `Smart` mode only engages when necessary, such as when a downstream Destination becomes blocked *and* the **Buffer size limit** reaches its limit. When persistent queue is set to `Smart` mode, Cribl attempts to flush the queue when every new event arrives. The only time events stay in the buffer is when a downstream Destination becomes blocked.
>
{.box .info}

**Buffer size limit**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. This buffer is for all connections, not just per Worker Process. For that reason, this can dramatically expand memory usage. Connections share this limit, which may result in slightly lower throughput for higher numbers of connections. For higher numbers of connections, consider increasing the limit.

**Commit frequency**: The number of events to send downstream before committing that Stream has read them. Defaults to `42`.

**File size limit**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, and so forth. If not specified, Cribl Stream applies the default `1 MB`.

**Queue size limit**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Source will stop queueing data and block incoming data. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, and so forth. Can be set as high as `1 TB`, unless you've [configured](group-settings#storage) a different **Worker Process PQ size limit** in Group or Fleet settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Stream will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file closes. Defaults to `None`; `Gzip` is also available.

> In Cribl Stream 4.1 and newer, the Source persistent queue default **Mode** is `Always on`, to best ensure events' delivery. For details on optimizing this selection, see [Optimize Source Persistent Queues (sPQ)](persistent-queues-sources).
>
> You can optimize Workers' startup connections and CPU load at **Group/Fleet settings** > [Worker Processes](group-settings#processes).
>
{.box .info}

### Processing Settings {#processing}

On this tab, you can enrich fields with added fields, and attach a pre-processing Pipeline to condition inbound syslog data.

#### Fields

In this section, you can define new fields or modify existing ones using JavaScript expressions, similar to the [Eval](eval-function) function. 

* The **Field Name** can either be a new field (unique within the event) or an existing field name to modify its value.
* The **Value** is a JavaScript expression (enclosed in quotes or backticks) to compute the field's value (can be a constant). Select this field's advanced mode icon (far right) if you'd like to open a modal where you can work with sample data and iterate on results.

This flexibility means you can:

* Add new fields to enrich the event.
* Modify existing fields by overwriting their values.
* Compute logic or transformations using JavaScript expressions.

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-settings}

**Enable proxy protocol**: Toggle on if the connection is proxied by a device that supports [PROXY protocol](https://www.haproxy.org/download/1.8/doc/proxy-protocol.txt) v1 or v2.

  * **Enable enhanced TLS handshake for proxy protocol**: Toggle on to make TLS handshakes compatible with the PROXY protocol v1 and v2. When enabled, PROXY headers are parsed during the TLS handshake, reliably preserving the original source IP and port even for encrypted traffic routed through proxies or load balancers that prepend these headers.

**Single msg per UDP**: Toggle on to treat received UDP packet data as a full syslog message. If toggled off (default) Cribl Edge will treat newlines within the packet as event delimiters.

**Allow non-standard app name**: Toggle on to allow hyphens to appear in an RFC 3164–formatted Syslog message's `TAG` section. For details, see [TAG Section Processing](#tag).

**Enable TCP load balancing**: Toggle on if you want the Source to load balance traffic across all Worker Processes, as explained in [TCP Load Balancing](#tcp-lb). This setting is available only in Cribl Stream deployed in Distributed mode and when a **TCP port** is configured.

**IP allowlist regex**: Regex matching IP addresses that are permitted to send data. Defaults to `.*` (that is, all IPs).

**Active connection limit**: Maximum number of active connections allowed per Worker Process. Defaults to `1000`. Set a lower value if connection storms are causing the Source to hang. Set `0` for unlimited connections.

**TCP socket idle timeout (seconds)**: The duration that Cribl Edge will wait for activity on an idle TCP socket before closing the connection. Disabled when set to `0`, the default.

**TCP forced socket termination timeout (seconds)**: The extra time the server waits before forcibly closing a socket that has been idle (**TCP socket idle timeout**) or exceeded its maximum lifespan (**TCP socket max lifespan**) but has not yet properly closed. This prevents resource leaks caused by unresponsive clients or network issues. Configure based on network latency and client behavior. Default: `30` seconds. Set to `0` to disable.

**TCP socket max lifespan (seconds)**: The duration that a socket is allowed to remain open, regardless of activity. This setting prevents resource exhaustion (such as TCP pinning) by limiting the lifespan of connections. Configure based on expected connection durations and resource availability. Disabled when set to `0`, the default.

**Buffer size limit (events)**: Maximum number of events to buffer when downstream is blocking. The buffer is only in memory. (This setting is applicable only to UDP syslog.) Events dropped because they exceed this buffer are logged as dropped in [`stats` messages](#stats).

**UDP socket buffer size (bytes)**: Optionally, set the SO_RCVBUF socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Min: `256`. Max: `4294967295`.

You might also need to increase the size of the buffer available to the `SO_RCVBUF` socket option. Consult the documentation for your operating system for a specific procedure.

> Setting this value will affect OS memory utilization.
>
{.box .warning}

**Default timezone**: Timezone to assign to timestamps that omit timezone info. Accept the default  `Local` value, or use the drop-down list to select a specific timezone by city name or GMT/UTC offset.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

#### TCP Load Balancing {#tcp-lb}

> This feature is available only for Cribl Stream deployments that are in Distributed mode. The UI displays the setting when a **TCP port** is configured.
>
{.box .warning}

When the Syslog Source receives a high volume of data over TCP, a single Worker Process can place excessive CPU load on its Cribl Stream Worker Node, while remaining Worker Processes on the same node mostly sit idle. This undesirable condition is called "TCP pinning" because the high volume of traffic "pins" a single TCP connection.

To alleviate TCP pinning, try toggling **Advanced Settings** > **Enable TCP load balancing** on. The Syslog Source will then load balance traffic across all Worker Processes.

When TCP load balancing is enabled, the Worker Node forks a special Worker Process dedicated to load balancing; creates sockets for communication between the load balancer and the regular Worker Processes; and, makes TCP load balancing metrics available.

##### The Load Balancing Process

The Worker Node forks a new, special **Load Balancer** Worker Process that distributes incoming syslog data among the other Worker Processes.
For customer-managed (hybrid or on-prem) Fleets, consider reducing the [Process count](/stream/scaling#worker-processes) by 1, if possible. This will leave a core free to run the load balancer process.

If you want to verify that the load balancer Worker Process exists, teleport into the relevant Worker and in the **Worker Processes** tab you will see a Worker Process with `LB` in its name.

To view logs from the Leader UI, navigate to **Monitoring** > **Logs** > `<Worker Node ID>` > **Load Balancer**.

##### Worker Process Socket Files

The load balancer Worker Process sends data to the regular Worker Processes over inter-process communication (IPC) sockets. This means that in the `state` directory, there is a socket file for each load balancer Worker Process and each Worker Process.

By default, the socket files are linked to `state`, from the Cribl `tmp` directory. For customer-managed (hybrid or on-prem) Stream Worker Groups, you can specify a directory other than `tmp` for these links. Cribl recommends that you do **not** use this option, but if you find it necessary, navigate to **Worker Group/Fleet Settings** > **General Settings** > **Sockets** to configure as desired.

##### TCP Load Balancing Metrics

Metrics specific to TCP load balancing become available.

* `lb.bytes_out` – Total bytes processed by the load balancer Worker Process, by Source.

* `lb.writable_sockets` – Total unblocked Worker Process sockets, by Source.

* `lb.blocked_sockets` – Total blocked Worker Process sockets, by Source.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source data via the Routing table.

Select **QuickConnect** to send the data from this Source to one or more Destinations via independent, direct connections.

## `TAG` Section Processing {#tag}

Cribl Edge will try to parse `appname` out of a syslog message's `TAG` section, even when the message is not RFC 5424–formatted.

This practice means that the `TAG` section can contain any alphanumeric character or an underscore (`_`). When the **Allow–non‑standard app name** option is enabled, Cribl Edge will also process hyphens that appear in an RFC 3164–formatted Syslog message's `TAG` section. If Cribl Edge encounters a hyphenated `appname`, it will continue processing to find `procid`. (This setting has no effect on RFC 5424–formatted messages.)

If the `TAG` section contains any non-alphanumeric character, Cribl Edge will treat that character as the termination of the `TAG` section, and as the starting character of the `CONTENT` section.

## `CONTENT` Section Processing {#content}

Cribl Edge will try to parse `procid` from the beginning of the `CONTENT` section if this section is directly after the `TAG` section, and if it either follows a `:` or is surrounded by `[]`. This process occurs regardless of the **Allow–non‑standard app name** setting.

## What Fields to Expect

To create fields, the Syslog Source requires messages to comply with RFC 3164 or RFC 5424. If messages sent to the Source comply with neither of these RFCs, you'll see the following fields:

- `_raw`: Contains the whole message, but no other fields broken out.
- `_time`: Contains the time the message was received by Cribl Edge.
- `__srcIpPort <udp|tcp>:<port>`: See [Internal Fields](#internal-fields).
- `__syslogFail`: See [Internal Fields](#internal-fields).
- `host`: IP address of the device sending the incoming message.

If messages sent to the Source comply with either RFC 3164 or RFC 5424, fields that the RFC deems guaranteed will always be there, but fields deemed optional might or might not be. Once Cribl Edge parses the required fields and any optional fields, what remains is the actual message.

To see this in real life, install the `cribl-syslog-input` Pack and preview the `RFC5424-RFC3164.log` sample file.

RFC Name for Field | Cribl Name for Field | Guaranteed? | Notes
--- | --- | --- | ---
`PRI` | `facility`, `facilityName`, `severity`, `severityName` | Yes | `PRI` is a bitwise representation of Facility and Severity, which is how Cribl Edge can derive values for all four of its fields.
`TIMESTAMP` | `_time` | Yes | This field's format differs depending on which RFC the messages adhere to. Cribl Edge converts it to UNIX epoch time.
`HOSTNAME` | `host` | Yes |
`APP-NAME` | `appname` | No |
`PROCID` | `procid` | No |
`MSGID` | `msgid` | No |
`STRUCTURED-DATA` | `structuredData` | No |
`MSG` | `message` | Yes

## Event Size and Storage

By default, a Syslog Source produces eight parsed fields: `_time`, `appname`, `facility` (numeric), `facilityName` (text), `host`, `message`, `severity` (numeric), and `severityName` (text). This improves readability but does not reduce event volume. It also introduces redundancy, numeric and text pairs for `facility` and `severity`.

If you keep all of the default fields (the default behavior), events written to downstream storage are larger than the original raw syslog line because the original message is accompanied by additional fields. For short messages, this overhead can be significant relative to the raw size and could roughly double the stored size. For larger messages, the percentage increase is lower.

To control size growth, configure trimming at the Source or in a Pipeline:
* Adjust the Source **Fields to keep** setting to include only the fields you require. By default, all fields are kept.
* Use a Pipeline to remove redundant/unnecessary fields and, if appropriate, shrink outbound `_raw` to only the message component.

For a walk‑through of optimization steps and expected results, see [Syslog Best Practices: The Goal—Optimizing Syslog Events](stream/usecase-syslog#the-goal-optimizing-syslog-events).

## Internal Fields {#internal-fields}

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but are accessible and [Functions](functions) can use them to make processing decisions.

Fields for this Source:

* `__inputId`
* `__srcIpPort <udp|tcp>:<port>`: Identifies the port of the sending socket.
* `__syslogFail`: `true` for data that fails RFC 3164/5424 validation as syslog format.



## `stats` Log Message {#stats}

In Cribl Edge 4.2.2 and newer, `stats` log messages report the number of events received, buffered, or dropped for exceeding the [maximum Cribl buffer size](#advanced-settings). By default, these messages are logged every 60 seconds. These values also appear in **Sources** > **Syslog** > <Source_name> > **Status**, in the **UDP** row.

## UDP Tuning {#udp-tuning}

This section applies only to UDP-based syslog ingest with on-prem or hybrid Cribl Stream Worker Groups or Cribl Edge Fleets. It does not apply to TCP-based syslog, nor to Cribl-managed Stream Workers in Cribl.Cloud, where Cribl will manage UDP tuning for you.

Incoming UDP traffic is put into a buffer by the Linux kernel. Cribl will drain from that buffer as resources are available. At lower throughput levels, and with plenty of available processes, this isn’t an issue. As you scale up, however, the default size of that buffer is very likely too small.

#### Monitoring Buffer Size and Packet Errors {#monitoring}

You can check the current buffer size with the following command:

```shell
$ sysctl net.core.rmem_max
```
A typical value of about 200 KB is far too small for an even moderately busy syslog server.

You can check the health of UDP with the following command. Check the `packet receive errors` line.

```shell
$ netstat -su
```

If `packet receive errors` is more than zero, you have lost events,
which is a particularly serious problem if the number of errors is increasing rapidly. This means you need to increase your `net.core.rmem_max` setting (covered later in this section).

#### Commands to Update Settings {#commands-settings}

You can update the live settings. But you’ll also need to change the boot-time setting, so that next time you reboot, everything is ready to roll.

Live change, setting to 25 MB:

```shell
$ sysctl -w net.core.rmem_max=26214400
net.core.rmem_max = 26214400
$ sysctl -w net.core.rmem_default=26214400
net.core.rmem_default = 26214400
```

For the permanent settings change, add the following lines to `/etc/sysctl.conf`:

```shell
net.core.rmem_max=26214400
net.core.rmem_default=26214400
```

We recommend that you track a few things related to UDP receiving:

- The `netstat -su` command, watching for errors.
- The **Status** tab in the Syslog Source. In particular, watch for dropped messages.
  They could indicate you need a bigger buffer under **Advanced Settings** (default: 1000 events). They could also indicate your Worker is encountering pressure further down the Pipeline.
- Especially if you increase your kernel receive buffer as above, watch your Worker processes' memory usage.

## Troubleshooting {#troubleshooting}

The Source's configuration modal has helpful tabs for troubleshooting:

**Live Data**: Try capturing [live data](sources#capture-source-data) to see real-time events as they are ingested. On the **Live Data** tab, click **Start Capture** to begin viewing real-time data.

**Logs**: Review and search the logs that provide detailed information about the ingestion process, including any errors or warnings that may have occurred.

You can also view the [Monitoring](monitoring) page that provides a comprehensive overview of data volume and rate, helping you identify ingestion issues. Analyze the graphs showing events and bytes in/out over time.

> [Cribl University](https://cribl.io/university/) offers an Advanced Troubleshooting > [Source Integrations: Syslog](https://university.cribl.io/#/online-courses/c1741eed-c361-4095-b66f-c8592e27da28) short course. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to review a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses and [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0).
{.box .course}
