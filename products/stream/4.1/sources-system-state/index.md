# System State


The System State Source collects snapshots of the host system's current state, on a configurable schedule, and sends them out as events to downstream systems for operational and security analytics. 

> Type: **System and Internal**  |  TLS Support: **N/A** | Event Breaker Support: **No**  {{< id `restrictions` >}}
> 
> Cribl Stream supports configuring only one System State Source per Worker Node and per Worker Group. This Source is currently unavailable on Cribl-managed Cribl.Cloud Workers.
>
{.box .info}

## Configuring Cribl Stream to Collect System State Events 

From the top nav, click **Manage**, then select a **Fleet** to configure. Then, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **System State**. By default, a **System State** tile appears at left. Hover over it and select **Configure** to open a drawer that provides the options below. 
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**System and Internal** >] **System State**. Next, click the default `in_system_state` Source to open modal that provides the options below.

### General Settings 

**Input ID**: This is prefilled with the default value `in_system_state`, which cannot be changed via the UI, due to the [single‑Source restrictions](#restrictions) above.

#### Optional Settings

**Polling interval**: How often, in seconds, to collect system state events. If not specified, defaults to `300s`.

> Each run of the state collector generates events with identical `_time` values.
>
{.box .info}

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Collector Settings

Cribl Stream contains the following System State collectors:
- [Host Info](#hostinfo)
- [DNS](#dns)
- [Disks and File Systems](#disk)
- [Hosts File](#hosts)
- [Interfaces](#interfaces)
- [Routes](#routes)
- [Users and Groups](#users-groups)

#### Host Info {#hostinfo}

With the **Enabled** slider on (the default), Cribl Stream will create events based on entries collected from the hosts file.

A partial list of Host Info events includes:

- host
- timestamp
- cribl.version
- cribl.mode
- cribl.group
- cribl.config_version
- os.arch
- os.cpu_count

See the **Explore** > **System State** > **Host Info** UI for a full list of events.

#### DNS {#dns}

With the **Enabled** slider on (the default), Cribl Stream will create events for DNS resolvers and search entries. The event field mapping depends on the operating system.

##### Linux

Cribl Stream emits events with the following fields:

| Event Field | Description |
| ---------- | ----------- | 
| `nameServer` | Name server entries from the file |
| `search` | Search entries from the file |
| `source` |The source of the data used to populate the event |

##### Windows

Cribl Stream emits events with the following fields:

 | Event Field | Description |
 | ----------- | ----------- |
| `ifAlias` | Interface alias name (if present) |
| `ifIndex` | Interface index (if present) |
| `addressFamily` | IPv4 IPv6 |
| `nameServer` | Array of name servers |
| `search` | Array of search suffixes |
| `source` |The source of the data used to populate the event |

#### Disks and File Systems {#disk}

With the **Enabled** slider on (the default), Cribl Stream will collect entries on physical disks, partitions, and mounted filesystems. These entries can help you monitor drive status, including available space, percent utilization, inode availability, wait times, and number of blocks free. 

This option is available for Windows and Linux Worker Groups, but you must create a different Worker Group for each OS. Follow this procedure:
1. Create a Worker Group for the appropriate OS.
1. Modify the Worker Group by disabling or removing the pre-defined Sources that do not apply to the OS. Remove Windows Event Forwarder or Windows Event Logs and Windows Metrics from Linux Worker Groups, and remove Linux-specific Sources from Windows Worker Groups.
1. Modify the Worker Group mapping to filter by platform – `platform=='win32'` for Windows or `platform=='linux'` for Linux.
1. Deploy Cribl Stream Worker Nodes and verify that the mapping is correct.
> If you are using the free version of Cribl Stream, you will only get a single default Worker Group. If you need more than one Worker Group, you'll have to limit your Cribl Stream deployment to one OS or upgrade to Cribl Stream Enterprise.
>
{.box .info}

For each physical disk, Cribl Stream will collect:
- name
- size (in blocks)
- blockSize (in bytes)
- model
- serial
- firmware
- partitions (count)
- interface type


For each partition on a physical disk, Cribl Stream will collect:
- disk (name)
- disk ID (number)
- type (primary or logical)
- label
- offset
- size (in blocks)
- blockSize (in bytes)
- filesystem (mountpoint)

For each mounted filesystem, Cribl Stream will collect:
- mountpoint (path)
- disk (name)
- partition (ID)
- filesystem type
- size (in blocks)
- blockSize (in bytes)
- blocksFree
- inodes
- inodesFree
- flags

#### Hosts File {#hosts}

With the **Enabled** slider on (the default), Cribl Stream will collect entries from the `hosts` file and emit them as events. 

Examples of entries in the `hosts` file are: 
  - `ip` – IP address of the host.
  - `hostnames` – list of hostnames per IP address.

#### Interfaces {#interfaces}

With the **Enabled** slider on (the default), Cribl Stream will create events for each of the host's network interfaces.

Cribl Stream will create separate events for each entry. All events will have identical timestamp values.

##### Linux IPV4/IPV6

On Linux, Cribl Stream creates `interface` events with the following fields:

- `ifIndex`: Interface index identification numbers.
- `ifName`: The name of the network interface.
- `flags`: Flags.
- `mtu`: Maximum transmission unit (MTU) in bytes for packets sent on this interface.
- `operstate`: Operational State. 
- `linkType`: Physical link type.
- `macAddress`: Media Access Control address.
- `broadcast` Broadcast address. 
- `addrInfo`: Array of `{family, ipAddress, mask, prefix}`.

#####  Windows: IPV4/IPV6
On Windows, Cribl Stream creates `interface` events with the following fields:

- `ifIndex`: Interface index identification numbers.
- `interface`: The name of the network interface.
- `mtu`: Maximum transmission unit (MTU) in bytes for packets sent on this interface.
- `flags`: Flags.
- `macAddress`: Media Access Control address.
- `addrInfo`: Array of `{family, ipAddress, mask, prefix}`.

#### Routes {#routes}

With the **Enabled** slider on (the default), Cribl Stream will collect entries from host's network routes and emit them as events.  

Sample entries include: 
- `address_family`– `ipv4` or `ipv6`
- `destination` – The destination network or host.
- `prefix`  –  Emits only from Linux IPV6.
- `flags`– Flags 
- `gateway`–The gateway address
- `interface` – Interface to which packets for this route will be sent.
- `ifIndex` – Interface ID. Emits only from Windows. 
- `mask`– The netmask for the destination net. Emits only from Linux IPV4. 
- `metric`– The distance to the target.
- `sequence` – Sequence number in the table.
- `source` – The source of the data used to populate the event.

#### Users and Groups {#users-groups}

With the **Enabled** slider on (the default), Cribl Stream will create events for users and groups.

##### Linux

On Linux, Cribl Stream creates `user` events with the following fields:
- `username`
- `fullname`
- `loginShell`
- `userHome`
- `userid`
- `primaryGroup` (name of primary group, not group ID)
- `groups` (list of group names)

Cribl Stream also creates `group` events with the following fields:
- `name`
- `groupId`
- `users`

##### Windows

On Windows, Cribl Stream creates `user` events with the following fields:
- `userName`
- `fullname`
- `description`
- `groups`

Cribl Stream also creates `group` events with the following fields:
- `name`
- `description`
- `users`

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

Select a **Pipeline** (or Pack) from the drop-down to process this Source's data. Required to configure this Source via **Data Routes**; optional to configure via **Collect**/**QuickConnect**.

#### Disk Spooling

**Enable disk spooling**: Whether to save metrics to disk. When set to `Yes`, exposes this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Stream will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`. 

**Max data age**: How long to retain data. Once reached, Cribl Stream will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Currently locked to `none`. Cribl plans to add (optional) disk-spooling compression in a future release.  

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/system_state`.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using [GitOps](/stream/GitOps), optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.