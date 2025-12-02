# System Metrics Source


Cribl Edge instances can collect metrics from their Linux hosts, and can
populate some standard metrics dashboards right out of the box. To see all of the metrics this Source supports, check out [Linux System Metrics
Details](/edge/system-metrics-linux-output).

> Type: **System**  |  TLS Support: **N/A** | Event Breaker Support: **No**
>
> Cribl Edge Workers support this System Metrics Source only when running on Linux. To collect metrics from Windows hosts, use the [Windows Metrics](/edge/sources-windows-metrics) Source.
> 
> You can't configure custom System Metrics Sources.
> 
{.box .info}

> This Source is available in Cribl.Cloud on customer-managed [hybrid Workers](cloud-enterprise#hybrid), but not on Cribl-managed Workers in Cribl.Cloud.
>
{.box .cloud}


## Configure Cribl Edge to Collect System Metrics

1. On the top bar, select **Products**, and then select **Cribl Edge**. Under **Fleets**, select a Fleet. Next, you have two options:
     - To configure via [QuickConnect](quickconnect), navigate to **Routing** > **QuickConnect** (Stream) or **Collect** (Edge). Select **Add Source** and select the Source you want from the list, then choose **Select Existing**.
     - To configure via the [Routes](routes), select **Data** > **Sources** (Stream) or **More** > **Sources** (Edge). Select the Source you want.
2. Next, select the default `in_system_metrics` to open the configuration modal.
3. Configure the following under **General Settings**:
   - **Enabled**: Toggle on to enable the Source.
   - **Input ID**: This is prefilled with the default value `in_system_metrics`, which cannot be changed via the UI.
   - **Description**: Optionally, enter a description.
4. Next, you can configure the following **Optional Settings**:
   - **Polling interval**: How often to collect metrics, in seconds. Defaults to `10`.
   - **Tags**: Optionally, add tags that you can use to filter and group Sources in Cribl Edge's UI. These tags aren't added to processed events. Use a tab or hard return between (arbitrary) tag names.
5. Optionally, you can configure metrics for [Host](#host), [Process](#process), [Container](#container), or adjust [Processing](#processing), [Advanced](#advanced-tab) settings and [Connected Destinations](#connected) outlined in the sections below.
6. Select **Save**, then **Commit & Deploy**.


### Host Metrics {#host}

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
  * **Per-interface metrics**, which toggle on or off.
  * **Detailed metrics**, which toggle on or off. If on, the **Protocol metrics** toggle appears, allowing you to choose whether to generate metrics for ICMP, ICMPMsg, IP, TCP, UDP, and UDPLite.

#### Disk

**Basic** level captures disk used in percent, bytes read and written, and read and write operations, over all mounted disks.

**Custom** exposes the following:
  * The **Device filter**, which specifies which block devices to include or exclude (all are included if the filter is empty). Wildcards and `!` (not) operators are supported.
  * The **Mountpoint filter**, which specifies which filesystem mountpoints to include or exclude (all are included if the filter is empty). Wildcards and `!` (not) operators are supported.
  * The **Filesystem type filter**, which specifies which filesystem types to include or exclude (all are included if the filter is empty). Wildcards and `!` (not) operators are supported.
  * **Per-device metrics**, which toggle on or off.
  * **Detailed metrics**, which toggle on or off. If on, the **Enable inode metrics** toggle appears, allowing you to choose whether to generate metrics for filesystem inodes.

### Process Metrics {#process}

With Process Metrics enabled, Cribl Edge captures process-specific metrics from
Linux servers and reports them as events. This allows you to monitor specific
processes on Cribl.Cloud instances. You can generate events for any process
object.

To collect a process metrics event, create a Process Set and add
a filter expression. Processes that match the filter are returned as individual
events. See [Collecting Metrics](#collecting-metrics). Process Sets are separate
from aggregate and host-wide metrics.

Process-specific metrics are **not** affected by the **Host Metrics** detail
setting.

#### Adding a Process Set

To add a Process Set:

1. Open the System Metrics Source and access the **Configure** tab.
1. Select the **Process Metrics** menu, then **Add Process Set**.
1. Configure the details:
   - **Set Name**: The name for this process set.
   - **Filter Expression**: The JavaScript expression that will filter the processes.
   - **Include Child Processes**: When toggled on, the processes that match the filter
      include metrics for child processes.

#### Filtering Processes

You can filter processes using the field names and values from each process
object.

> ##### Tip
>
> To see all processes currently running on an Edge Node, select
> **Explore** in the submenu and open the [**Processes Tab**](/edge/explore-edge-linux#processes).
>
{.box .success}

##### Example filters

This filter looks for Java processes in the sleep state (an
interruptible wait) that are using more than 100% of the CPU or more than 25%
of memory:

`cmdline.args[0] == 'java' && status == 'S' && (cpu > 100 || memory_percent > 25)`

This filter looks for running NGINX web servers that have more than 12 open file descriptors:

`service == 'nginx' && fds > 12`

Here are some more Linux processes you could find metrics for:

- `cmdline.args[0].endsWith('/java') && cmdline.args[1] == 'nifi'`
- `exe.path == '/usr/bin/sshd'`
- `uid == 2`
- `cgroup.0 == /user.slice/user-1000.slice/`
- `stat.status == R`
- `stat.starttime > 943517`
- `cpu > 30`
- `memory_percent > 30`
- `stat.num_threads > 10`

> The **Filter expression** field expects a specific syntax in order to apply
> the filter to the processes. These expressions evaluate to either `true` or
> `false`. See
> [Build Custom Logic to Route and Process Your Data: Filter Expressions](filter-and-transform-data#filters) for more information.
>
{.box .info}

#### Collecting Metrics

Once you have at least one Process Set created and saved, Cribl Edge will begin
collecting process metrics at the interval defined in the **Polling interval**
field (**General Settings** menu).

To view the status of the Collector and total events collected, click the
**Status** tab in the Linux Metrics Source.

To view a live capture of individual metrics gathered from processes that match
the Process Set, click the **Live Data** tab. The Process Set that owns each
metric is represented by the `__process_set` internal field.

#### Supported Processes

To view the complete table of supported Linux process metrics, their descriptions, types,
and dimensions, see [Process-Specific Metrics](/edge/system-metrics-linux-output#process-metrics).

#### Using Advanced Mode

To test your filter expression against a sample input, click the **Advanced
mode** button within the **Filter expression** field. The **Filter expression**
modal will open. See [Test and Validate Expressions in Advanced Mode](filter-and-transform-data#advanced-mode)
for more information.

### Container Metrics {#container}

Use the buttons to select a level of detail.

* **Basic** generates the server event and per running container events.
* **All** adds per-device and detailed metrics.
* **Custom** provides controls for customizing which containers to generate metrics from.
* **Disabled** means that no metrics will be generated.

The **Custom** buttons displays additional controls, as follows:

* **Container filters**: Enter a filter expression to govern which containers to generate metrics from. Leave empty (the default) to generate metrics from all containers.
* **All containers**: Toggle on to include stopped and paused containers.
* **Per-device metrics**: Toggle on to generate separate metrics for each device.
* **Detailed metrics**: Toggle on to generate full container metrics.

The **Basic**, **All**, and **Custom** buttons provide the following **Advanced Settings**, which are different from those in the main **Advanced Settings** [tab](#advanced-tab):

* **Docker socket**: Enter the full path(s) for Docker's UNIX domain socket. Defaults to `/var/run/docker.sock` and `/run/docker.sock`.
* **Docker timeout**: Timeout, in seconds, for the Docker API. Defaults to `5`.

### Processing Settings {#processing}

#### Fields

[Snippet not found: content/shared/4.13/snippets/_sources-add-fields.md]

#### Pre-Processing

If desired, choose a **Pipeline** from the drop-down if you want to process data from this Source before sending it through the Routes.

#### Disk Spooling

**Enable disk spooling**: Whether to save metrics to disk. When toggled on, exposes this section's remaining fields.

**Bucket time span**: The amount of time that data is held in each bucket before it’s written to disk. The default is 10 minutes (`10m`).

**Data size limit**: Maximum disk space the persistent metrics can consume. Once reached, Cribl Edge will delete older data. Example values: `420 MB`, `4 GB`. Default value: `100 MB`.

**Data age limit**: How long to retain data. Once reached, Cribl Edge will delete older data. Example values: `2h`, `4d`. Default value: `24h` (24 hours).

**Data compression format**: Optionally compress the data before sending. Defaults to `gzip` compression. Select `none` to send uncompressed data.

**Path location**: Path to write metrics to. Default value is `$CRIBL_HOME/state/system_metrics`.

### Advanced Settings {#advanced-tab}

**Environment**: If you're using GitOps, optionally use this field to specify a single Git branch on which to enable this configuration. If empty, the config will be enabled everywhere.

### Connected Destinations {#connected}

Select **Send to Routes** to enable conditional routing, filtering, and cloning of this Source's data via the Routing table.

Select **QuickConnect** to send this Source’s data to one or more Destinations via independent, direct connections.

## Populating Dashboards with System Metrics

Cribl has configured a Prometheus dashboard to display Cribl Edge System Metrics, and [shared](https://grafana.com/grafana/dashboards/15280) the dashboard in the Grafana library, which is public. This is a relatively simple dashboard suitable for showing aggregate metrics. Try this dashboard if you prefer **Basic** mode for most of your metrics.

Another useful dashboard is the one that's commonly used with Prometheus and their Node Exporter agent, found [here](https://grafana.com/grafana/dashboards/1860). This dashboard can handle the highly detailed metrics. To use this dashboard, attach the `prometheus_metrics` pre-processing pipeline, and choose **All** mode.

For details on how populate your Grafana Cloud dashboards with system metrics, see [System Metrics to Grafana](/stream/usecase-metrics-grafana).
