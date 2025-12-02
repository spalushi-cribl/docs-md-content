# System State


The System State Source collects snapshots of the host system's current state, on a configurable schedule, and sends them out as events to downstream systems for operational and security analytics. 

> Type: **System**  |  TLS Support: **N/A** | Event Breaker Support: **No**  {{< id `restrictions` >}}
> 
> Cribl Edge supports configuring only one System State Source per Edge Node and per Fleet.
>
{.box .info}

> This Source is currently unavailable on Cribl-managed Cribl.Cloud Workers.
>
{.box .cloud}

## Configuring Cribl Edge to Collect System State Events 

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

**Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's **Manage Sources** page. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.

### Collector Settings

Cribl Edge contains the following System State collectors:
- [Host Info](#hostinfo)
- [Disks and File Systems](#disk)
- [DNS](#dns)
- [Firewall](#firewall)
- [Hosts File](#hosts)
- [Interfaces](#interfaces)
- [Listening Ports](#listening)
- [Logged-In Users](#users)
- [Routes](#routes)
- [Services](#services)
- [Users and Groups](#users-groups) 

#### Host Info {#hostinfo}

With the **Enabled** toggle on (the default), Cribl Edge will create events based on entries collected from the hosts file.

A partial list of Host Info events includes:

- `host`
- `timestamp`
- `cribl.version`
- `cribl.mode`
- `cribl.group`
- `cribl.config_version`
- `os.arch`
- `os.cpu_count`

See the **Explore** > **System State** > **Host Info** UI for a full list of events.

#### Disks and File Systems {#disk}

With the **Enabled** toggle on (the default), Cribl Edge will collect entries on physical disks, partitions, and mounted filesystems. These entries can help you monitor drive status, including available space, percent utilization, inode availability, wait times, and number of blocks free. 

This option is available for Windows and Linux Fleets, but you must create a different Fleet for each OS. Follow this procedure:
1. Create a Fleet for the appropriate OS.
1. Modify the Fleet by disabling or removing the pre-defined Sources that do not apply to the OS. Remove Windows Event Forwarder or Windows Event Logs and Windows Metrics from Linux Fleets, and remove Linux-specific Sources from Windows Fleets.
1. Modify the Fleet mapping to filter by platform – `platform=='win32'` for Windows or `platform=='linux'` for Linux.
1. Deploy Cribl Edge Edge Nodes and verify that the mapping is correct.

> If you are using the free version of Cribl Edge, you will only get a single default Fleet. If you need more than one Fleet, you'll have to limit your Cribl Edge deployment to one OS or upgrade to Cribl Edge Enterprise.
>
{.box .info}

For each physical disk, Cribl Edge will collect:
- name
- size (in blocks)
- blockSize (in bytes)
- model
- serial
- firmware
- partitions (count)
- interface type

For each partition on a physical disk, Cribl Edge will collect:
- disk (name)
- disk ID (number)
- type (primary or logical)
- label
- offset
- size (in blocks)
- blockSize (in bytes)
- filesystem (mountpoint)

For each mounted filesystem, Cribl Edge will collect:
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

#### DNS {#dns}

With the **Enabled** toggle on (the default), Cribl Edge will create events for DNS resolvers and search entries. The event field mapping depends on the operating system.

##### Linux

Cribl Edge emits events with the following fields:

| Event Field | Description |
| ---------- | ----------- | 
| `nameServer` | Name server entries from the file. |
| `search` | Search entries from the file. |
| `source` |The source of the data used to populate the event. |

##### Windows

Cribl Edge emits events with the following fields:

 | Event Field | Description |
 | ----------- | ----------- |
| `ifAlias` | Interface alias name (if present). |
| `ifIndex` | Interface index (if present). |
| `addressFamily` | IPv4 or IPv6. |
| `nameServer` | Array of name servers. |
| `search` | Array of search suffixes .|
| `source` |The source of the data used to populate the event. |


#### Hosts File {#hosts}

With the **Enabled** toggle on (the default), Cribl Edge will collect entries from the `hosts` file and emit them as events. 

Examples of entries in the `hosts` file are: 
  - `ip`: IP address of the host.
  - `hostnames`: list of hostnames per IP address.

#### Firewall {#firewall}

With the **Enabled** toggle on (the default), Cribl Edge will collect events from host’s defined firewall rules.

##### Chain Policy/References Rule(s) Event Fields

Cribl Edge emits events with the following fields:
- `chain`: Name.
- `policy`: Policy name.
- `pkts`: Total bytes packets processed by the chain.
- `bytes`: Total bytes processed by the chain.
- `references`: Number of chains referencing the current chain.

##### Chain Rule(s) Event Fields

Cribl Edge emits events with the following fields:

- `chain`: Name.
- `num`: Rule sequence number.
- `pkts`: Number of matched messages processed.
- `bytes`: Cumulative packet size processed (bytes).
- `target`: If the message matches the rule, the specified target is executed.
- `prot`: The specified protocol can be one of `tcp`, `udp`, `udplite`, `icmp`, `icmpv6`, `esp`, `ah`, `sctp`, `mh`, or the special keyword `all`.
- `opt`: Rarely used, this column is used to display IP options.
- `in`: Inbound network interface.
- `out`: Outbound network interface.
- `src`: The source IP address or subnet of the traffic, the latter being anywhere.
- `dest`: The destination IP address or subnet of the traffic, or anywhere.
- `match`: `ctstate`, `state`, `ADDRTYPE` info added to a state field.
- `comment`: Comment defined on the rule.
- `prot_family`: `IPv4` or `IPv6`.

##### Windows

On Windows, this collector emits events with the following fields: 

- `ruleName`: Rule Name.
- `displayName`: Display Name. 
- `direction`: Specifies which direction of traffic to match with this rule. The acceptable values for this parameter are `Inbound` or` Outbound`.
- `enabled`: `True` or `False`. 
- `action`: Specifies the action to take on traffic that matches this rule. The acceptable values for this parameter are `Allow` or `Block`.
- `group`: Specifies the rule group (if applicable).
- `icmpType`: Specifies the `ICMP` type used. 
- `policyStoreSource`: Contains a path to the policy store where the rule originated.
- `profile`: Profile conditions associated with a rule.
- `protocol`: protocols associated with firewall and `IPsec` rules. 
- `localPort`:  Rules for local-only port mapping.
- `remotePort`: Rules for remote port mapping.


#### Interfaces {#interfaces}

With the **Enabled** toggle on (the default), Cribl Edge will create events for each of the host's network interfaces.

Cribl Edge will create separate events for each entry. All events will have identical timestamp values.

##### Linux IPV4/IPV6

On Linux, Cribl Edge creates `interface` events with the following fields:

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
On Windows, Cribl Edge creates `interface` events with the following fields:

- `ifIndex`: Interface index identification numbers.
- `interface`: The name of the network interface.
- `mtu`: Maximum transmission unit (MTU) in bytes for packets sent on this interface.
- `flags`: Flags.
- `macAddress`: Media Access Control address.
- `addrInfo`: Array of `{family, ipAddress, mask, prefix}`.

#### Listening Ports {#listening}

With the **Enabled** toggle on (the default), Cribl Edge will create events from the list of listening ports and their associated process identifier (pid).

Cribl Edge will create separate events for each entry. All events will have identical timestamp values.

Sample entries include:

- `Protocol Family`: `ipv4` or `ipv6`.
- `Protocol`: `TCP`, `UDP`, or `UNIX`.
- `Socket`: `${addr}:${port}`, or `${path}` for UNIX sockets.
- `Process id`: The process identifier.
- `Program`: The program listening on the port.

#### Logged-In Users {#users}

With the **Enabled** toggle on (the default), Cribl Edge will collect entries from currently logged-in users.

##### Linux

Cribl Edge emits events with the following fields: 

- `uid`: User ID.
- `Last Login date`: Last login date. 
- `terminal`: Type of the terminal device.
- `hostname`: Display the hostname of the system, if the user is connected from a remote computer.
- `userName`: Logon name of the user.

##### Windows 

Cribl Edge emits events with the following fields: 

- `userName`: Login name of the user.
- `sessionName`: `rdp` (Remote desktop/terminal services login session) or `console` (Direct login session).
- `id`: Session ID.
- `state`: `active` (Active session) or `disc` (Disconnected/inactive session).
- `logonTime`: Date and time the user logged on.
- `source`: Set to `Query User`. 

#### Routes {#routes}

With the **Enabled** toggle on (the default), Cribl Edge will collect entries from host's network routes and emit them as events.  

Sample entries include: 
- `address_family`: `ipv4` or `ipv6`.
- `destination`: The destination network or host.
- `prefix`: Emits only from Linux IPv6.
- `flags`: Flags.
- `gateway`: The gateway address.
- `interface`: Interface to which packets for this route will be sent.
- `ifIndex`: Interface ID. Emits only from Windows. 
- `mask`: The netmask for the destination net. Emits only from Linux IPv4. 
- `metric`: The distance to the target.
- `sequence`: Sequence number in the table.
- `source`: The source of the data used to populate the event.

#### Services {#services}

With the **Enabled** toggle on (the default), Cribl Edge will create events for each configured service (e.g. `systemd` and `initd`) along with their enabled and running status.

Sample entries include:
- `Name`: Name of the service.
- `Unit Activation Status`: The high-level unit activation state, i.e. generalization of `SUB` for `systemctl`. For `sysV` system value can be `enabled` or `disabled`.
- `Unit Load Status`: Reflects whether the unit definition was properly loaded. (Only `systemctl`).
- `Sub Unit Activation Status`: The low-level unit activation state, values depend on unit type.
- `Description`: Description of the service. 

#### Users and Groups {#users-groups}

With the **Enabled** toggle on (the default), Cribl Edge will create events for users and groups.

##### Linux

On Linux, Cribl Edge creates `user` events with the following fields:
- `username`
- `fullname`
- `loginShell`
- `userHome`
- `userid`
- `primaryGroup` (name of primary group, not group ID)
- `groups` (list of group names)

Cribl Edge also creates `group` events with the following fields:
- `name`
- `groupId`
- `users`

##### Windows

On Windows, Cribl Edge creates `user` events with the following fields:
- `userName`
- `fullname`
- `description`
- `groups`

Cribl Edge also creates `group` events with the following fields:
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

**Bucket time span**: What time range of events to hold in each bucket. Default value: `10m` (10 minutes). 

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Edge will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`. 

**Max data age**: How long to retain data. Once reached, Cribl Edge will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Currently locked to `none`. Cribl plans to add (optional) disk-spooling compression in a future release.  

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/system_state`.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using [GitOps](/stream/GitOps), optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.