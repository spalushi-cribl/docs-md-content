# System State


The System State Source collects snapshots of the host system's current state, on a configurable schedule, and sends them out as events to downstream systems for operational and security analytics. 

> Type: **System and Internal**  |  TLS Support: **N/A** | Event Breaker Support: **No**  {{< id `restrictions` >}}
> 
> Cribl Stream supports configuring only one System State Source per Worker Node and per Worker Group. This Source is currently unavailable on Cribl-managed Cribl.Cloud Workers.
>
{.box .info}

## Configuring Cribl Stream to Collect System State Events 

From the top nav, click **Manage**, then select a **Fleet** to configure. Then, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **+ Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **System State**. By default, a **System State** tile appears at left. Hover over it and select **Configure** to open a drawer that provides the options below. 
 
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

#### Hosts File {#hosts}

With the **Enabled** slider on (the default), Cribl Stream will collect entries from the `hosts` file and emit them as events.

Examples of entries in the `hosts` file are: 
  - `ip` – IP address of the host.
  - `hostnames` – list of hostnames per IP address.

#### Routes Collector {#routes}

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