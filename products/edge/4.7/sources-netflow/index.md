# NetFlow Source




Cribl Edge supports receiving NetFlow v5 data via UDP.

> Type: **Push**  |  TLS Support: **No** | Event Breaker Support: **No**
>
{.box .info}

This Source ingests NetFlow records similarly to how it ingests events from other upstream senders: fields are broken out, and the message header is included with each record. If you prefer to render NetFlow data as metrics, use a pre-processing Pipeline or a Route.

If you want to associate a [Community ID](https://github.com/corelight/community-id-spec) with a network flow that passes through this Source, connect the Source to a Route containing a Function that supports expression evaluation; then, in the Function, use the [C.Net.communityIDv1()](/stream/expressions-net/#cnetcommunityidv1) method. You can even generate different CommunityIDs for different use cases within the same network flow – to do this, use multiple `communityIDv1()` methods, with the optional `seed` parameter.   

## Configuring Cribl Edge to Receive NetFlow Data

From the top nav, click **Manage**, then select a **Fleet** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**Push** > ] **NetFlow**. Next, click either **+ Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**Push** > ] **NetFlow**. Next, click **Add Source** to open a **New Source** modal that provides the options below.

> Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
> This results from restrictions of the UDP protocol.
> 
> To minimize the risk of data loss, deploy a hybrid Stream Worker Group
> with customer-managed Worker Nodes as close as possible to the UDP sender.
> Cribl also recommends tuning the OS UDP buffer size.
>
{.box .cloud}

### General Settings

**Input ID**: Enter a unique name to identify this NetFlow Source definition. 

**Address**: Enter the hostname/IP to listen for NetFlow data. For example: `localhost`, `0.0.0.0`, or `::`.

**Port**: Enter the port number.

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Persistent Queue Settings {#pq}

In this section, you can optionally specify [persistent queue](persistent-queues) storage, using the following controls. This will buffer and preserve incoming events when a downstream Destination is down, or exhibiting backpressure.

> On Cribl-managed [Cribl.Cloud](deploy-cloud) Workers (with an [Enterprise plan](cloud-enterprise)), this tab exposes only the **Enable Persistent Queue** toggle. If enabled, PQ is automatically configured in `Always On` mode, with a maximum queue size of 1 GB disk space allocated per PQ‑enabled Source, per Worker Process. 
> 
> The 1 GB limit is on uncompressed inbound data, and no compression is applied to the queue. This limit is not configurable. For configurable queue size, compression, mode, and other options below, use a [hybrid Group](cloud-enterprise#hybrid).
>
> For more about PQ modes, see [Always On versus Smart Mode](persistent-queues-configuring#source-pq-busy).
>
{.box .cloud}

**Enable Persistent Queue**: Defaults to `No`. When toggled to `Yes`:

**Mode**: Select a condition for engaging persistent queues.
- `Always On`: This default option will always write events to the persistent queue, before forwarding them to Cribl Edge's data processing engine.
- `Smart`: This option will engage PQ only when the Source detects backpressure from Cribl Edge's data processing engine.

**Max buffer size**: The maximum number of events to hold in memory before reporting backpressure to the sender and writing the queue to disk. Defaults to `1000`. (This buffer is per connection, not just per Worker Process – and this can dramatically expand memory usage.)

**Commit frequency**: The number of events to send downstream before committing that Cribl Edge has read them. Defaults to `42`.

**Max file size**: The maximum data volume to store in each queue file before closing it and (optionally) applying the configured **Compression**. Enter a numeral with units of KB, MB, etc. If not specified, Cribl Edge applies the default `1 MB`.

**Max queue size**: The maximum amount of disk space that the queue is allowed to consume on each Worker Process. Once this limit is reached, this Source will stop queueing data and block incoming data. Required, and defaults to `5` GB. Accepts positive numbers with units of `KB`, `MB`, `GB`, etc. Can be set as high as `1 TB`, unless you've [configured](settings-group-fleet#storage) a different **Max PQ size per Worker Process** in Fleet Settings.

**Queue file path**: The location for the persistent queue files. Defaults to `$CRIBL_HOME/state/queues`. To this field's specified path, Cribl Edge will append `/<worker-id>/inputs/<input-id>`.

**Compression**: Optional codec to compress the persisted data after a file is closed. Defaults to `None`; `Gzip` is also available.
 
> You can optimize Workers' startup connections and CPU load at **Fleet Settings** > [Worker Processes](settings-group-fleet#processes).
>
{.box .info}

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 
  * **Name**: Field name.
  * **Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing Pipeline to process data from this input before the data is sent through the Routes.

### Advanced Settings

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection. Defaults to `.*` (that is, all IPs).

**IP denylist regex**: Regex matching IP addresses whose messages you want this Source to ignore. Defaults to `^$` (that is, every specific IP address in the list). This takes precedence over the allowlist.

**UDP socket buffer size (bytes)**: Optionally, set the `SO_RCVBUF` socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Minimum: `256`. Maximum: `4294967295`.

It may also be necessary to increase the size of the buffer available to the `SO_RCVBUF` socket option. Consult the documentation for your operating system for a specific procedure.

> Setting this value will affect OS memory utilization.
>
{.box .warning}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## What Fields to Expect

The NetFlow Source does minimal processing of the incoming UDP messages to remain consistent with the internal Cribl [event model](event-model). For each UDP message received on the socket, you can expect an event with the following fields. We have organized these fields into categories to make them easier to grasp; the categories are our own, and not from the NetFlow specifications. The field definitions are mostly copied from Cisco NetFlow [documentation](https://www.cisco.com/c/en/us/td/docs/net_mgmt/netflow_collection_engine/5-0-3/user/guide/format.html#wp1006108).

#### General 

* `_time`: The UNIX timestamp (in seconds) at which the message was received by Cribl Edge.
* `source`: A string in the form `udp|<remote IP address>|<remote port>`, indicating the remote sender.
* `host`: The hostname of the machine running Cribl Edge that ingested this event.
* `inputInt`: SNMP index of input interface; always set to zero.  
* `outputInt`: SNMP index of output interface.
* `protocol`: IP protocol type (for example, TCP = 6; UDP = 17) of the observed network flow.
* `tcpFlags`: Cumulative logical OR of TCP flags in the observed network flow.
* `tos`: IP type of service; switch sets it to the ToS of the first packet of the flow.
* `icmpType`: Type of the ICMP message. Only present if the protocol is ICMP.
* `icmpCode`: Code of the ICMP message. Only present if the protocol is ICMP.

Because this Source reads input data directly from bytes in a compact format, there is nothing suitable to put in a `_raw` field, and the Source does not add a `_raw` field to events.

#### Flow Source and Destination

* `srcAddr`: Source IP address; in case of destination-only flows, set to zero.
* `dstAddr`: Destination IP address.
* `nextHop`: IP address of next hop router.
* `srcPort`: TCP/UDP source port number. 
* `dstPort`: TCP/UDP destination port number. If this is an ICMP flow, this field is a combination of ICMP type and ICMP code, which are broken out separately as `icmpType` and `icmpCode` fields.
* `srcMask`: Source address prefix mask bits.
* `dstMask`: Destination address prefix mask bits.
* `srcAs`: Autonomous system number of the source, either origin or peer.
* `dstAs`: Autonomous system number of the destination, either origin or peer.

#### Flow Statistics

* `packets`: Packets in the flow.
* `octets`: Total number of Layer 3 bytes in the packets of the flow. 
* `startTime`: System uptime, in milliseconds, at the start of the flow. 
* `endTime`: System uptime, in milliseconds, at the time the last packet of the flow was received.
* `durationMs`: `endTime` minus `startTime`.

#### Header

The following are subfields within the `header` field:

* `count`: Number of flows exported in this flow frame (protocol data unit, or PDU).
* `sysUptimeMs`: Current time in milliseconds since the export device booted
* `unixSecs`: Current seconds since 0000 UTC 1970.
* `unixNsecs`: Residual nanoseconds since 0000 UTC 1970.
* `flowSequence`: Sequence counter of total flows seen.
* `engineType`: Type of flow switching engine.
* `engineId`: ID number of the flow switching engine.
* `samplingMode`: The first two bits of what Cisco NetFlow documentation calls `SAMPLING_INTERVAL`. These bits specify a sampling mode.
* `samplingInterval`: The remaining 14 bits of what Cisco NetFlow documentation calls `SAMPLING_INTERVAL`. These bits specify a sampling interval.
* `version`: NetFlow export format version number.

Also, the internal fields listed below will be present.

## Internal Fields

Cribl Edge uses a set of internal fields to assist in handling of data. These "meta" fields are **not** part of an event, but they are accessible, and [functions](functions) can use them to make processing decisions.

Fields accessible for this Source:

  * `__bytes`
  * `__inputId`
  * `_time`

## UDP Tuning

Incoming UDP traffic is put into a buffer by the Linux kernel. Cribl will drain from that buffer as resources are available.
At lower throughput levels, and with plenty of available processes, this isn’t an issue.
As you scale up, however, the default size of that buffer may be too small.

You can check the current buffer size with:

```shell
$ sysctl net.core.rmem_max
```

A typical value of about 200 KB is far too small for an even moderately busy syslog server.
You can check the health of UDP with the following command. Check the `packet receive errors` line.

```shell
$ netstat -su
```

If `packet receive errors` is more than zero, you have lost events,
which is a particularly serious problem if the number of errors is increasing rapidly.
This means you need to increase your `net.core.rmem_max` setting (see earlier).

You can update the live settings, but you’ll also need to change the boot-time setting so next time you reboot everything is ready to roll.

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

We recommend you track a few things related to UDP receiving:

- The `netstat -su` command, watching for errors.
- The **Status** tab in the UDP (Raw) Source. In particular, watch for dropped messages.
  They could indicate you need a bigger buffer under **Advanced Settings** (default: 1000 events). They could also indicate your Worker is encountering pressure further down the Pipeline.
- Especially if you increase your kernel receive buffer as above, watch your Worker processes' memory usage.

## Troubleshooting

[Snippet not found: content/shared/4.7/snippets/_sources-troubleshooting.md]
