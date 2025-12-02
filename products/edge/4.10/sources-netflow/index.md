# NetFlow & IPFIX Source


Cribl Edge supports receiving NetFlow v5, v9, and IPFIX (v10) data via UDP.

> Type: **Push**  |  TLS Support: **No** | Event Breaker Support: **No**
>
{.box .info}

This Source ingests NetFlow records similarly to how it ingests events from other upstream senders: fields are broken out, and each record includes a message header. If you prefer to render NetFlow or IPFIX data as metrics, use a pre-processing Pipeline or a Route.

If you want to associate a [Community ID](https://github.com/corelight/community-id-spec) with a network flow that passes through this Source, connect the Source to a Route containing a Function that supports expression evaluation; then, in the Function, use the [C.Net.communityIDv1()](/stream/expressions-net/#cnetcommunityidv1) method. You can even generate different CommunityIDs for different use cases within the same network flow – to do this, use multiple `communityIDv1()` methods, with the optional `seed` parameter.

> Sending large numbers of UDP events per second can cause Cribl.Cloud to drop some of the data.
> This results from restrictions of the UDP protocol.
>
> To minimize the risk of data loss, deploy a hybrid Stream Worker Group
> with customer-managed Worker Nodes as close as possible to the UDP sender.
> Cribl also recommends tuning the OS UDP buffer size.
>
{.box .cloud}

## Configure a NetFlow & IPFIX Source

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
      - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, choosing either **Select Existing** or **Add New**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want. Next, select **Add Source**.
2. In the **New Source** modal, configure the following under **General Settings**:
    - **Input ID**: Enter a unique name to identify this Source.
    - **Description**: Optionally, enter a description.
    - **Address**: Enter the hostname/IP on which to listen for data. For example: `localhost`, `0.0.0.0`, or `::`.
    - **Port**: Enter the port number.
    - **Enable pass-through**: Enable this setting to allow Cribl Edge to forward events to a NetFlow Destination. If enabled, Cribl Edge generates an additional event containing `__netflowRaw`, which directly routes the raw NetFlow packet data to a downstream NetFlow Destination. These additional events do not count against any ingest quotas.
    - **Version Support**: Toggle which versions of NetFlow and IPFIX (NetFlow v10) data are processed. By default, all versions (`V5`, `V9`, and `IPFIX`) are enabled. You can disable support for specific versions if needed. Disabling a version prevents the Source from processing data for that version and treats any received packets of that version as unsupported messages.
3. Next, you can configure the following **Optional Settings**:
    - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
4. Optionally, you can adjust the [Persistent Queue](#pq), [Processing](#processing), and [Advanced](#advanced-settings) settings outlined in the sections below.
5. Select **Save**, then **Commit & Deploy**.

### Persistent Queue Settings {#pq}

[Snippet not found: content/shared/4.10/snippets/_sources-persistent-queue-settings.md]

### Processing Settings {#processing}

#### Fields

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality.
  * **Name**: Field name.
  * **Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

In this section's **Pipeline** drop-down list, you can select a single existing [Pipeline](pipelines) or [Pack](packs) to process data from this input before the data is sent through the Routes.

### Advanced Settings {#advanced-settings}

**IP allowlist regex**: Regex matching IP addresses that are allowed to establish a connection. Defaults to `.*` (that is, all IPs).

**IP denylist regex**: Regex matching IP addresses whose messages you want this Source to ignore. Defaults to `^$` (that is, every specific IP address in the list). This takes precedence over the allowlist.

**UDP socket buffer size (bytes)**: Optionally, set the `SO_RCVBUF` socket option for the UDP socket. This value tells the operating system how many bytes can be buffered in the kernel before events are dropped. Leave blank to use the OS default. Minimum: `256`. Maximum: `4294967295`.

It may also be necessary to increase the size of the buffer available to the `SO_RCVBUF` socket option. Consult the documentation for your operating system for a specific procedure.

> Setting this value will affect OS memory utilization.
>
{.box .warning}

**Template cache minutes (minutes)**: This setting only affects v9 flow records. Use this setting to specify the length of time (in minutes) that Cribl Edge caches NetFlow v9 templates before discarding them. For example, setting the duration to `30` means Cribl Edge will cache templates for 30 minutes. If flow records are not refreshed by a new event, the templates are discarded.

The template cache minutes setting helps optimize the performance and reliability of NetFlow v9 data processing. This setting manages memory usage and ensures that outdated templates do not persist indefinitely. The goal is to balance system performance against memory use. Monitor your network and adjust this setting to find the right balance between template availability and system data overload. Some considerations when tuning your system:

* If your network's data patterns change often, consider using a shorter cache duration. Conversely, if the data records are stable and change less frequently, try using a longer duration.
* Monitor the status of this Source across your Worker/Edge nodes. If you notice Cribl Edge discards FlowSets, it may mean the cache duration is too short. Try increasing the cache duration and then observer whether there is a performance improvement. Alternatively, adjust your NetFlow exporters to send templates more frequently.
* If your system is using too much memory, it may mean that your cache duration is too long, especially if you have a lot of NetFlow exporters sending data to a single Source.

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## What Fields to Expect

The NetFlow Source does minimal processing of the incoming UDP messages to remain consistent with the internal Cribl [event model](event-model). For each UDP message received on the socket, you can expect an event with the following fields.

### Field Representation for NetFlow v9 & IPFIX (v10)

For both NetFlow v9 and IPFIX (v10), fields map to their most natural representation in Cribl. Specifically:

* IPv4 addresses are represented as dotted octet strings.
* IPv6 as colon-separated octet strings.
* MAC addresses also as colon-separated strings.
* Numbers that are six bytes or less are represented as numbers.
* Numbers that are greater than six bytes are represented as strings.
* Registered field attribute names are the camelCase versions of the SCREAMING_CASE names in the [Cisco whitepaper](https://www.cisco.com/en/US/technologies/tk648/tk362/technologies_white_paper09186a00800a3db9.html).

Cribl Edge collects and offers unregistered fields in an array of records consisting of their type codes and byte buffers. You can add Pipeline functions to change how these fields are processed by default.

### Fields for NetFlow IPFIX (v10)

IPFIX fields are derived from the [IANA IPFIX Information Elements](https://www.iana.org/assignments/ipfix/ipfix.xhtml#ipfix-information-elements) list. These fields are predefined and included in the [ipfix-information-elements.yml](ipfix-information-elementsyml) configuration file.

### Fields for NetFlow v9

We have organized these fields into categories to make them easier to grasp; the categories are our own, and not from the NetFlow specifications. The field definitions are mostly copied from Cisco NetFlow [documentation](https://www.cisco.com/c/en/us/td/docs/net_mgmt/netflow_collection_engine/5-0-3/user/guide/format.html#wp1006108).

Not all NetFlow v9 fields will appear on all events, only those that are specified by the template for that specific data record. These fields are available for NetFlow v9:

* `inBytes`: The number of incoming Bytes.
* `inPkts`: The number of incoming packets.
* `flows`: The number of flows that were aggregated.
* `protocol`: The IP protocol.
* `srcTos`: The type of service setting when entering the incoming interface.
* `tcpFlags`: The accumulation of all the TCP flags seen in this flow.
* `l4SrcPort`: The TCP/UDP source port number.
* `ipv4SrcAddr`: The IPv4 source address.
* `srcMask`: The number of contiguous bits in the source subnet mask.
* `inputSnmp`: The input interface index.
* `l4DstPort`: The TCP/UDP destination port number.
* `ipv4DstAddr`: The IPv4 destination address.
* `dstMask`: The number of contiguous bits in the destination subnet mask.
* `outputSnmp`: The output interface index.
* `ipv4NextHop`: The IPv4 address of the next-hop router.
* `srcAs`: The source BGP autonomous system number. The length may be and defaults to 2.
* `dstAs`: The destination BGP autonomous system number. The length may be and defaults to 2.
* `bgpIpv4NextHop`: The IPv4 address of the next-hop router in the bgp domain.
* `mulDstPkts`: The number of IP multicast outgoing packets.
* `mulDstBytes`: The number of IP multicast outgoing bytes.
* `lastSwitched`: The system uptime in milliseconds at which the last packet of this flow was switched.
* `firstSwitched`: The system uptime in milliseconds at which the first package of this flow was switched.
* `outBytes`: The number of outgoing bytes.
* `outPkts`: The number of outgoing packets.
* `minPktLngth`: The minimum IP packet length on incoming packets.
* `maxPktLngth`: The maximum IP packet length on incoming packets.
* `ipv6SrcAddr`: The IPv6 source address.
* `ipv6DstAddr`: The IPv6 destination address.
* `ipv6SrcMask`: The number of contiguous bits in the IPv6 source mask.
* `ipv6DstMask`: The number of contiguous bits in the IPv6 destination mask.
* `ipv6FlowLabel`: The IPv6 flow label as per the RFC 2460 definition.
* `icmpType`: The ICMP packet type, reported as type * 256 + code.
* `mulIgmpType`: The IGMP packet type.
* `samplingInterval`: The rate at which packets are sampled; a value of 100 indicates that 1% of packets are sampled.
* `samplingAlgorithm`: The platform sampling algorithm; 1 is deterministic, 2 is random.
* `flowActiveTimeout`: Timeout value in seconds for active flow entries in the NetFlow cache.
* `flowInactiveTimeout`: Timeout value in seconds for inactive flow entries in the NetFlow cache.
* `engineType`: The type of flow switching engine; 0 is RP, 1 is VIP/Linecard.
* `engineId`: The ID number of the flow switching engine.
* `totalBytesExp`: The number of bytes exported by the observation domain.
* `totalPktsExp`: The number of packets exported by the observation domain.
* `totalFlowsExp`: The number of flows exported by the observation domain.
* `ipv4SrcPrefix`: The IPv4 source address prefix, specific for Catalyst architecture.
* `ipv4DstPrefix`: The IPv4 destination address prefix, specific for Catalyst architecture.
* `mplsTopLabelType`: The MPLS top label type; 0 is unknown, 1 is TE midpoint, 2 is AToM, 3 is VPN, 4 is BGP, and 5 is LDP.
* `mplsTopLabelIpAddr`: The IPv4 address of the forwarding equivalent class corresponding to the MPLS top label.
* `flowSamplerId`: The ID number of the flow sampler.
* `flowSampleMode`: The type of algorithm used for sample data.
* `flowSamplerRandomInterval`: The packet interval at which to sample.
* `minTtl`: Minimum TTL on incoming packets.
* `maxTtl`: Maximum TTL on incoming packets.
* `ipv4Ident`: The IPv4 identification field.
* `dstTos`: The type of service byte setting when exiting the outgoing interface.
* `inSrcMac`: The incoming source MAC address.
* `outDstMac`: The outgoing destination MAC address.
* `srcVlan`: The virtual LAN ID associated with the ingress interface.
* `dstVlan`: The virtual LAN ID associated with the egree interface.
* `ipProtocolVersion`: The IP version. The default is assumed to be 4 if unspecified.
* `direction`: The flow direction; 0 is ingress, 1 is egress.
* `ipv6NextHop`: The IPv6 address of the next-hop router.
* `bgpIpv6NextHop`: The IPv6 address of the next-hop router in the bgp domain.
* `ipv6OptionHeaders`: The IPv6 option headers found in the flow.
* `mplsLabel1`: The MPLS label at position 1 in the stack.
* `mplsLabel2`: The MPLS label at position 2 in the stack.
* `mplsLabel3`: The MPLS label at position 3 in the stack.
* `mplsLabel4`: The MPLS label at position 4 in the stack.
* `mplsLabel5`: The MPLS label at position 5 in the stack.
* `mplsLabel6`: The MPLS label at position 6 in the stack.
* `mplsLabel7`: The MPLS label at position 7 in the stack.
* `mplsLabel8`: The MPLS label at position 8 in the stack.
* `mplsLabel9`: The MPLS label at position 9 in the stack.
* `mplsLabel10`: The MPLS label at position 10 in the stack.
* `inDstMac`: The incoming destination MAC address.
* `outSrcMac`: The outgoing source MAC address.
* `ifName`: The shortened interface name.
* `ifDesc`: The full interface name.
* `samplerName`: The name of the flow sampler.
* `inPermanentBytes`: The running byte counter for a permanent flow.
* `inPermanentPkts`: The running packet counter for a permanent flow.
* `fragmentOffset`: The fragment-offset value from fragmented IP packets.
* `forwardingStatus`: The forwarding status.
* `mplsPalRd`: The MPLS pal route distinguisher.
* `mplsPrefixLen`: The number of consecutive bits in the MPLS prefix length.
* `srcTrafficIndex`: The BGP policy accounting source traffic index.
* `dstTrafficIndex`: The BGP policy accounting destination traffic index.
* `applicationDescription`: The application description.
* `applicationTag`: The application tag.
* `applicationName`: The name associated with a classification.
* `postIpDiffServeCodePoint`: The value of a differentiated services code point encoded in the differentiated services field, after modification.
* `replicationFactor`: The multicast replication factor.
* `layer2PacketSectionOffset`: The layer 2 packet section offset, potentially a generic offset.
* `layer2PacketSectionSize`: The layer 2 packet section size, potentially a generic size.
* `layer2PacketSectionData`: The layer 2 packet section data.

#### Option Data Records for NetFlow v9

Options data records report aggregate metadata about the Exporters. Like data records, they can include any or all of the previous fields. Additionally, they also have a `scopes` field with a value containing an array of records with a `type` field. The valid types are:

* `system`
* `interface`
* `line-card`
* `cache`
* `template`

Option data records also have a `value` field, which is a natural number. The JavaScript type is either a number or a string, depending on the size of the value.

### Fields for NetFlow v5

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
* `sysUptimeMs`: Current time in milliseconds since the export device booted.
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

  * `__netflowRaw` : Directly routes the raw NetFlow packet data to a downstream NetFlow Destination.


## Considerations for Working with NetFlow Data FlowSets

For both NetFlow v5 and v9, Cribl Edge:

* Can forward NetFlow packets to other NetFlow destinations. However, it cannot modify the the contents of the incoming packet. In other words, Cribl Edge forwards the packets verbatim as they came in.
* Only routes NetFlow packets from upstream exporters and cannot generate its own NetFlow packets.
* Cannot forward non-NetFlow input data to NetFlow Destinations.
* Can forward NetFlow packets to non-NetFlow Destinations, such as Splunk, Syslog, S3, and others. To send NetFlow flow records to other non-NetFlow Destinations, use the other events generated on the NetFlow & IPFIX Source that do not include the `__netflowRaw` field. NetFlow Destinations discard those events.

Cribl Edge emits:

* Decoded Cribl events downstream for each data record and options data record.
* When you enable pass-through mode: an undecoded Cribl event for the entire packet.

## High Availability and Disaster Recovery for NetFlow v9

NetFlow v9 Template Records are sent every 20 packets or every 30 minutes. Cribl Edge uses a key-value store to share these records across Edge Nodes in a Fleet, ensuring availability for processing FlowSets.

* **Template Sharing**: Records are shared across Edge Nodes with a minimal delay, ensuring timely processing. The brief interval between receiving and sharing Template Records is designed to be negligible, given the low rate of Template Record changes.
* **Worker Coordination**: The key-value store allows uninterrupted processing during Edge Node failures.
* **Disaster Recovery**: Replication in the key-value store ensures Template Records are preserved, enabling seamless recovery.

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

[Snippet not found: content/shared/4.10/snippets/_sources-troubleshooting.md]

### Data Collisions

When Cribl Edge receives a NetFlow v9 template, it identifies the template using three values:

* Template ID
* Source ID, which is unique to the upstream exporting device
* IP address of the upstream exporting device

Cribl Edge uses these three values to prevent data collisions. However, data collisions may occur under these two conditions:

* Your system has a heterogeneous variety of upstream exporter devices, AND
* The Exporter IP addresses are rewritten (such as by a proxy server) before they are sent downstream to Cribl Edge.

In that situation, it is possible for two exporter devices with the same IP address to send packets simultaneously and cause a collision. The workaround is to create unique IP addresses for the exporters or use the original IP addresses.
