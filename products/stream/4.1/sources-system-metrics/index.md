# System Metrics


<head>
<meta name="keywords" content="Edge Grafana, Cribl Edge grafana dashboard, grafana Edge, grafana cribl" />
</head>
 

Cribl Stream can collect metrics from the host on which it is running, and can populate some standard metrics dashboards right out of the box. For metrics details, see [Linux System Metrics Details](/edge/system-metrics-linux-output). 

> Type: **System and Internal**  |  TLS Support: **N/A** | Event Breaker Support: **No**
> 
> Cribl Edge Workers support System Metrics only when running on Linux, not on Windows.
> 
> This Source is not available on Cribl-managed Cribl.Cloud Workers.
>
{.box .info}

## Configuring Cribl Stream to Collect System Metrics

From the top nav, click **Manage**, then select a **Worker Group** to configure. Next, you have two options:

To configure via the graphical [QuickConnect](quickconnect) UI, click Routing > QuickConnect (Stream) or Collect (Edge). Next, click **Add Source** at left. From the resulting drawer's tiles, select [**System and Internal** >] **System Metrics**. Next, click either **Add Destination** or (if displayed) **Select Existing**. The resulting drawer will provide the options below.
 
Or, to configure via the [Routing](routes) UI, click **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). From the resulting page's tiles or left nav, select [**System and Internal** >] **System Metrics**.  Next, click **New Source** to open a **New Source** modal that provides the options below.

### General Settings

**Input ID**: Enter a unique name to identify this Source definition.

### Optional Settings

**Polling interval**: How often to collect metrics, in seconds. Defaults to `10`.

**Tags**: Optionally, add tags that you can use for filtering and grouping in Cribl Stream. Use a tab or hard return between (arbitrary) tag names.

### Host Metrics

Use the buttons to select a level of detail.

* **Basic** enables minimal metrics, averaged or aggregated.
* **All** enables full, detailed metrics, specified for individual CPUs, interfaces, and so on.
* **Custom** displays sub-menus and buttons from which you can choose a level of detail (**Basic**, **All**, **Custom**, or **Disabled**) for each type of event.
* **Disabled** means that no metrics will be generated.

The meaning of **All** and **Disabled** are self-evident. **Basic** and **Custom** have different meanings depending on event type, as follows: 

#### System

**Basic** level captures load averages, uptime, and CPU count.

**Custom** toggles **Process** metrics on or off; these are metrics for the numbers of processes in various states.

#### CPU

**Basic** level captures active, user, system, idle, iowait percentages over all CPUs.

**Custom** toggles the following on or off: **Per CPU metrics**, **Detailed metrics** (meaning, for all CPU states), and **CPU time metrics** (meaning raw, monotonic CPU time counters).

#### Memory

**Basic** level captures captures total, used, available, `swap_free`, and `swap_total`.

**Custom** toggles **Detailed metrics** on or off. Detailed means for all memory states.

#### Network

**Basic** level captures bytes, packets, errors, connections over all interfaces.

**Custom** exposes the following:
  * The **Interface filter**, which specifies which network interfaces to include or exclude (all are included if the filter is empty).
  * **Per interface metrics**, which toggle on or off.
  * **Detailed metrics**, which toggle on or off. If on, the **Protocol metrics** toggle appears, allowing you to choose whether to generate metrics for ICMP, ICMPMsg, IP, TCP, UDP, and UDPLite.

#### Disk

**Basic** level captures disk used in percent, bytes read and written, and read and write operations, over all mounted disks.

**Custom** exposes the following:
  * The **Device filter**, which specifies which block devices to include or exclude (all are included if the filter is empty). Wildcards and `!` (not) operators are supported.
  * The **Mountpoint filter**, which specifies which filesystem mountpoints to include or exclude (all are included if the filter is empty). Wildcards and `!` (not) operators are supported.
  * The **Filesystem type filter**, which specifies which filesystem types to include or exclude (all are included if the filter is empty). Wildcards and `!` (not) operators are supported.
  * **Per device metrics**, which toggle on or off.
  * **Detailed metrics**, which toggle on or off. If on, the **Enable inode metrics** toggle appears, allowing you to choose whether to generate metrics for filesystem inodes.

### Container Metrics

Use the buttons to select a level of detail.

* **Basic** generates the server event and per running container events.
* **All** adds per-device and detailed metrics.
* **Custom** provides controls for customizing which containers to generate metrics from.
* **Disabled** means that no metrics will be generated.

The **Custom** buttons displays additional controls, as follows:

* **Container Filters**: Enter a filter expression to govern which containers to generate metrics from. Leave empty (the default) to generate metrics from all containers.
* **All containers**: Toggle to `Yes` to include stopped and paused containers.
* **Per device metrics**: Toggle to `Yes` to generate separate metrics for each device.
* **Detailed metrics**: Toggle to `Yes` to generate full container metrics.

The **Basic**, **All**, and **Custom** buttons provide the following **Advanced Settings**, which are different from those in the main **Advanced Settings** [tab](#advanced-tab):

* **Docker socket**: Enter the full path(s) for Docker's UNIX domain socket. Defaults to `/var/run/docker.sock` and `/run/docker.sock`.
* **Docker timeout**: Timeout, in seconds, for the Docker API. Defaults to `5`.

### Processing Settings

#### Fields 

In this section, you can add Fields to each event using [Eval](eval-function)-like functionality. 

**Name**: Field name.

**Value**: JavaScript expression to compute field's value, enclosed in quotes or backticks. (Can evaluate to a constant.)

#### Pre-Processing 

If desired, choose a **Pipeline** from the drop-down if you want to process data from this Source before sending it through the Routes.

#### Disk Spooling

**Enable disk persistence**: Whether to save metrics to disk. When set to `Yes`, exposes this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Max data size**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Stream will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`. 

**Max data age**: How long to retain data. Once reached, Cribl Stream will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Compression**: Optionally compress the data before sending. Defaults to `gzip` compression. Select `none` to send uncompressed data.

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/system_metrics`.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Populating Dashboards with System Metrics

Cribl has configured a Prometheus dashboard to display Cribl Stream System Metrics, and [shared](https://grafana.com/grafana/dashboards/15280) the dashboard in the Grafana library, which is public. This is a relatively simple dashboard suitable for showing aggregate metrics. Try this dashboard if you prefer **Basic** mode for most of your metrics.

Another useful dashboard is the one that's commonly used with Prometheus and their Node Exporter agent, found [here](https://grafana.com/grafana/dashboards/1860). This dashboard can handle the highly detailed metrics. To use this dashboard, attach the `prometheus_metrics` pre-processing pipeline, and choose **All** mode.  

For details on how populate your Grafana Cloud dashboards with system metrics, see [System Metrics to Grafana](/stream/usecase-metrics-grafana).
