# Windows Metrics


Cribl Edge collects metrics from Windows hosts on which it runs, and can populate some standard metrics dashboards right out of the box. This Source produces events compatible with the [Prometheus Windows Exporter](https://github.com/prometheus-community/windows_exporter) to send out system, CPU, memory, network, and disk metrics.

> Type: **System and Internal**  |  TLS Support: **N/A** | Event Breaker Support: **No** {{< id `restrictions` >}}
> 
> Edge Nodes support Windows Metrics only when running on Windows. (To collect metrics from Linux hosts, use the [System Metrics](/stream/sources-system-metrics) Source.) Edge Leaders support configuring only one Windows Metrics Source per Fleet.
>
{.box .info}

## Configuring Cribl Edge/Windows to Collect System Metrics

From the top nav, click **Manage**, then select a **Fleet** to configure. Then, you have two options:

- To configure via the graphical [QuickConnect](quickconnect) UI, click **Collect**. By default, a **Windows Metrics** tile appears at left. Hover over it and select **Configure** to open a drawer that provides the options below. 

- To configure via the [Routing](routes) UI, click **More** > **Sources**. From the resulting page's tiles or the **Sources** left nav, select **System and Internal** > **Windows Metrics**. Next, click the default `in_windows_metrics` Source to open a modal that provides the options below. 

### General Settings

**Input ID**: This is prefilled with the default value `in_windows_metrics`, which cannot be changed via the UI, due to the [single‑Source restriction](#restrictions) above. 

### Optional Settings

**Polling interval**: How often, in seconds, to collect metrics. Defaults to `10` seconds.

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Edge. Use a tab or hard return between (arbitrary) tag names.

### Host Metrics

Use the buttons to select a level of detail:

* **Basic** enables minimal metrics, averaged or aggregated. This is the minimum configuration needed to support the Cribl Edge landing page. 
* **All** enables full, detailed metrics for individual CPUs, interfaces, etc.
* **Custom** displays submenus and buttons from which you can choose a level of detail (**Basic**, **All**, **Custom**, or **Disabled**) for each specific type of event.
* **Disabled** generates no metrics.

The meanings of **All** and **Disabled** are self-evident. **Basic** and **Custom** have different meanings depending on event type – see the following subsections.

##### System

**Basic** level captures load averages, uptime, and CPU count.

**Custom** level toggles **Detailed** metrics on or off. These are Windows-specific metrics including OS information, system uptime, CPU architecture, etc.

##### CPU

**Basic** level captures active, user, system, idle, and iowait percentages over all CPUs.

**Custom** level toggles the following on or off: **Per CPU metrics**, **Detailed metrics** (i.e., metrics for all CPU states), and **CPU time metrics** (i.e., raw, monotonic CPU time counters).

#### Memory

**Basic** level captures captures total, used, available, `swap_free`, and `swap_total`.

**Custom** level toggles **Detailed metrics** on or off. (These are metrics for all memory states.)

##### Network

**Basic** level captures bytes, packets, errors, and connections over all interfaces.

**Custom** level exposes the following:
  * The **Interface filter**, which specifies which network interfaces to include or exclude. (An empty filter will include all metrics.)
  * **Per interface metrics**, which toggle on or off.
  * **Detailed metrics**, which toggle on or off. If on, the **Protocol metrics** toggle appears, allowing you to choose whether to generate metrics for ICMP, ICMPMsg, IP, TCP, UDP, and UDPLite.

##### Disk

**Basic** level captures disk usage (%), bytes read and written, and read and write operations, over all mounted disks.

**Custom** level exposes the following:
  * The **Volume filter**, specifying which Windows volumes to include or exclude. Supports wildcards and `!` (not) operators. An empty filter will include all volumes.
  * **Per volume metrics**, which toggle on or off.
  * **Detailed metrics**, which toggle on or off. 
  
### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing

Select a **Pipeline** (or Pack) from the drop-down to process this Source's data. Required to configure this Source via **Data Routes**; optional to configure via **Collect**/**QuickConnect**.

#### Disk Spooling

**Enable disk persistence**: Whether to save metrics to disk. When set to `Yes`, exposes this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Edge will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`. 

**Max data age**: How long to retain data. Once reached, Cribl Edge will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Optionally compress the data before sending. Defaults to `gzip` compression. Select `none` to send uncompressed data.

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/windows_metrics`.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using [GitOps](/stream/GitOps), optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.


