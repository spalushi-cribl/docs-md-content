# Monitoring


To get an operational view of a Cribl Stream deployment, you can consult the following resources.

## Monitoring Resources

- [Monitoring Page](#monitoring-page)
- [Internal Logs and Metrics](#internal-logs-and-metrics)
- [Licensing](#licensing)
- [Flows](#flows)
- [Health Endpoint](#health-endpoint)

### Monitoring Page

From Cribl Stream's top nav, select **Monitoring** to access multiple dashboards. The resulting **Monitoring** page displays information about traffic in and out of the system, as well as collection jobs and tasks. It tracks events, bytes, splits by data fields over time, and broader system metrics. 

The initial view (below) shows aggregate data for all Worker Groups and all Workers. You can use the drop-downs at the upper right to isolate individual Groups, or individual Workers. 

Also at the upper right, you can change the display's granularity from the default `15 min`, selecting from a variety of time ranges from `1 min` up to `1 day`. (The latter covers the preceding 24 hours, and this maximum window is not configurable.)

The displayed **Storage** tile (upper right) represents the amount of free storage remaining on the partition where Cribl Stream is installed. (This quantity might not represent the maximum storage available for the selected Worker or Group. Also, it does not calculate the system free space.)

Similarly, the **Free Memory** graph reflects only the operating system's `free` statistic, matching Linux's strict `free` definition by excluding `buff/cache` memory. So this graph indicates a lower value than the OS' `available` memory statistic – and it does not necessarily indicate that the OS is running out of memory to allocate.

![Monitoring page](monitoring-page-4.1.02ecf07b1b.png)
{border="true"}

> Metrics are displayed in the Cribl Stream UI in units of KB, MB, GB, and TB. At each level, these are multiples of `1024`.
>
{.box .info}

Byte-related charts show the uncompressed amounts and rates of data processed over the selected time range: 
- Events (total) in and out
- Events per second in and out
- Bytes (total) in and out
- Bytes per second in and out

We measure bytes in based on the size of `_raw`, if this field is present and is of type `string`. Otherwise, we use the size of read (uncompressed) data. Bytes out is measured by the full serialized size of the payload.

The displayed **CPU Load Average** is an average per Worker Process, updated at 1‑minute granularity. (It is not an average for the Worker Node as a whole.)

Vertical lines across each chart display configuration changes. Click anywhere on the line to view summary information including time, data, and configuration versions. 

Select **View CPU load by node** to open a modal showing CPU usage per Worker. 

![Monitoring page](monitoring-cpu-usage.8b3dc9a00b.png)
{border="true"}

Select a **View Details** button to access a Worker Node's **Worker Settings**, with details per Worker Process. (To enable this button, you must first enable the parent Group's [Remote UI Access](deploy-distributed#worker-access).)

![Monitoring page](monitoring-worker-settings.b0b3333b2f.png)
{border="true"}

> Except for these configuration change markers, Monitoring data does not persist across Cribl Stream restarts. Keep this in mind before you restart the server.
>
{.box .warning}

#### Data Monitoring

From the **Monitoring** page, open the **Data** submenu to isolate throughput for any of the following:

- Data Fields
- Destinations
- Packs
- Pipelines
- Projects
- Routes
- Sources
- Subscriptions

![Monitoring > Data submenu](monitoring-data-submenu.1e00abf4e0.png)
{scale="70%" border="true"}

##### Sparklines

Dense displays are condensed to sparklines for legibility. Hover over the right edge to display Maximize buttons that you can click to zoom these up to detailed graphs.

![Sparklines and fly-out](Sparklines-composite.5ff9682ab5.png)

##### Data Fields

The **Data Fields** page lets you preview the flow of events that contain a specific data field.
The left (blue) side summarizes events in/out. The right (orange) side summarizes bytes in/out.

You can control which data fields are included in these graphs
with the [Disable field metrics](#criblmetrics-override) setting.

To add data fields to this setting, navigate to **Settings**, then select
**Global Settings**. From here, select **General Settings**, then **Metrics**
(under **Limits**).

![Data Fields graph in Monitoring](datafields-dashboard.png)
{border="true"}

> **Disable field metrics** settings are not available on Cribl.Cloud-managed Worker Nodes.
>
{.box .cloud}

##### Routes

The **Routes** page condenses multiple details and options:

- Each row independently summarizes throughput for a separate Route.
- The left (blue) side summarizes events in/out. The right (orange) side summarizes bytes in/out.
- On each row, the top number summarizes average events/bytes into the Route, and the bottom number summarizes events/bytes out. Both are averaged over your selected time granularity.
- On each row, the upper Maximize button zooms up the left (events) graph, and the lower Maximize button zooms up the right (bytes) graph.

![Routes page's multiple rows and controls](routes-dashboard.f33087cd5e.png)
{border="true"}

##### Fly-Out Details


You can hover over graphs to display a fly-out with details. On the **Free Memory** and **CPU Load Average** graphs, fly-outs are limited to a manageable 10 Worker Nodes, even if more Nodes are active. These fly-outs will occasionally show transient `0` metrics for some Workers, because Cribl Stream prioritizes reporting current throughput over memory/load metrics.

![Throughput details](monitoring-details-popup.b14d5114c6.png)
{border="true"}

#### System Monitoring

From the **Monitoring** page, open the **System** submenu to isolate throughput for any of the following:

- [Job Inspector](#job-inspector)
- Jobs (and tasks in-flight, see [Collector Sources](collectors))
- Leaders (available only with High Availability enabled; see [Second Leader](deploy-add-second-leader))
- [Licensing](#licensing)
- [Queues](persistent-queues) (Destinations)
- [Queues](persistent-queues) (Sources)

![Monitoring > System submenu](monitoring-system-submenu.f99a4dc622.png)
{scale="70%" border="true"}

##### Job Inspector {#job-inspector}

Select **System** then **Job Inspector** from the Monitoring page to view and manage pending, in-flight, and completed collection jobs and their tasks. For details about the resulting page, see [Monitoring and Inspecting Collection Jobs](collectors#inspect).

##### Leaders

This menu item is available only in Distributed Deployments with High Availability (HA) enabled. 

Select **Monitoring**, then **System** and **Leaders** to view the status of your Leader Nodes. For more information on how to configure a second Leader Node for failover/durability, see [Second Leader](deploy-add-second-leader)).

![Monitoring Leader Nodes](se-monitoring-leaders.ecce85b7d7.png)
{border="true"}

##### Licensing

Select **System**, then **Licensing** from the Monitoring page to check the expiration dates of your licenses, daily data throughput, and events quotas. You can also compare your daily Cribl Stream and Cribl Edge data throughput against your license quota – and against granular and average throughput over the last 30 to 365 days.

In this graph, you'll see the following:

- A horizontal bar that indicates your quota limit.
- Legacy data is represented by blue bars that show licensing usage in versions older than 4.6.0. Starting in v.4.6.0, the system separated the data, displaying Cribl Edge usage in teal and Cribl Stream usage in orange. If only Cribl Stream is used, Cribl continues to display usage with blue bars.
- Tooltips display details about data usage, data amount over/under license quota, Cribl Edge dropped bytes, and data percentage over/under license quota.
- Dots on the daily usage bar graph represent configuration changes in the   system.
- The **Daily Events In** chart only shows events that count towards your license, broken out by product (Cribl Stream and Cribl Edge). It filters out `datagen`, `cribl_http`, and `cribl_tcp` events.

![Licensing breakdown from the Monitoring page](stream-edge-licensing.png)
{border="true"}

> Even on single-instance deployments, you must have `git` installed in order for the [Monitoring](#monitoring) > **Licensing** page to display configuration change markers.
> 
> For the most current and accurate throughput data, enable the [Cribl Internal](sources-cribl-internal) Source's CriblMetrics option. Forward metrics to your metrics Destination of choice, and run a report on `cribl.total.in_bytes`. CriblMetrics aggregates metrics at the Worker Process level, every 2 seconds.
>
{.box .warning}

####  Reports/Top Talkers  {#talkers}

Select **Monitoring** > **Reports / Top Talkers** > **Top Talkers**, where you can examine your five highest-volume Sources, Destinations, Pipelines, Routes, and Packs. All components are ranked by events throughput. Sources and Destinations get separate rankings by bytes in and out, respectively.

![Monitoring > Reports/Top Talkers](st-monitoring-top-talkers.d245aa6a0f.png)
{border="true"}

#### Flows {#flows}

Select **Flows** from the Monitoring page's top nav or ••• overflow menu to see a graphical, left-to-right visualization of data flow through your Cribl Stream deployment.

#### Internal Logs and Metrics

Select **Logs** from the Monitoring page's top nav. Cribl Stream's [internal logs](#types) and [internal metrics](internal-metrics) provide comprehensive information about an instance's status/health, inputs, outputs, Pipelines, Routes, Functions, and traffic.

### Health Endpoint {#endpoint}

Each Cribl Edge instance exposes a `/health` endpoint that is commonly used along with a Load Balancer to support operational decision-making. See [Leader High Availability/Failover: Load Balancers](deploy-add-second-leader#loadbalancers) for more information.

> For many HTTP-based [Sources](sources), you can enable a Source-level health check endpoint in the **Advanced Settings** tab. The request URL format for these endpoints is `http(s)://${hostName}:${port}/cribl_health`. Refer to the configuration instructions for a Source to learn whether the health check endpoint is available for it. 
>
{.box .info}

## Types of Logs  {#types}

Cribl Stream provides the following log types, by originating process:

* API Server Logs – These logs are emitted primarily by the `API/main` process. They correspond to the top-level `cribl.log` that shows up on the [Diag](/stream/diagnosing) page. These include telemetry/license-validation logs. Filesystem location: `$CRIBL_HOME/log/cribl.log`

* Worker Process(es) Logs – These logs are emitted by all the Worker Processes, and are very common on single-instance deployments and Workers. Filesystem location: `$CRIBL_HOME/log/worker/N/cribl.log`

* Worker Group/Fleet Logs – These logs are emitted by all processes that help a Leader Node configure Worker Groups/Fleets. Filesystem location: `$CRIBL_HOME/log/group/GROUPNAME/cribl.log`

In a distributed deployment ([Stream](/stream/deploy-distributed)), all Workers forward their metrics to the Leader Node, which then consolidates them to provide a deployment-wide view.

> For details about generated log files, see [Internal Logs](internal-logs). To work with logs in Cribl Stream's UI, see [Search Internal Logs](#log_search).
>
{.box .success}

## Log Rotation and Retention {#retention}



Cribl Stream rotates logs every 5 MB, keeping the most recent 5 logs. Logs will reach the 5 MB threshold more quickly with verbose [logging levels](#levels) (such as `debug`) and with high volumes of system activity.

For long-term retention, Cribl recommends sending logs from the Leader, and from Workers' [Cribl Internal](sources-cribl-internal#configuring) > CriblLogs Sources, to a downstream service of your choice. See the next section for details.

## Forward Logs and Metrics Externally {#external-monitoring}

Cribl Stream supports forwarding internal logs and metrics to your preferred external monitoring solution. The [Cribl Internal](sources-cribl-internal) Source captures internal logs and metrics to send to [Destinations](destinations).

In the following steps, we'll use the graphical [QuickConnect](quickconnect) UI to set up the **Cribl Internal** Source and how data is sent to your Destination. See [Cribl Internal](sources-cribl-internal) for details on how to instead configure the **Cribl Internal** Source via the **Data** > **Sources** (Stream) or **More** > **Sources** (Edge) submenus.

1. From the top nav, click **Manage**, then select a **Worker Group** to configure.
1. To open **QuickConnect**:
  Stream – select the **QuickConnect** tile on the **Overview** page.
  Edge – click the **Collect** submenu.
1. Click **Add Source**.
1. From the resulting drawer's tiles, select **System and Internal**, then hover over the **Cribl Internal** tile.
1. Click **Select Existing**.
1. On the **CriblLogs** and/or the **CriblMetrics** row, toggle **Enabled** to `Yes`. Confirm your choice in the resulting message box.

  ![Cribl Internal Sources – click to configure](se-toggle-cribl-internal.06bef86eb4.png)
  {border="true"}

1. Notice how the **Routes/QC** column still says **Routes**. We want to use QuickConnect, so we'll change this by clicking on the **CriblLogs** and/or the **CriblMetrics** row again. Once clicked, you'll confirm **Yes** to the message box asking to switch the Source to send to QuickConnect instead of Routes.
1. Click and drag the **+** button on the right side of the Source to your desired Destination. A **Connection Configuration** modal will prompt you to select a **Passthru**, **Pipeline**, or **Pack connection**. See [QuickConnect](quickconnect#bottom-drawer) for details.

  ![Cribl Internal forwarding to Sumo Logic](se-forward-internal-logsMetrics-qc.f932bc90a1.png)
  {border="true"}

1. This will send internal logs and metrics to your Destination, just like another data Source. Both logs and metrics will have a field called `source`, set to the value `cribl`, which you can use in Routes' filters.

Note that the only logs supported here are Worker Process logs (see [Types of Logs](#types) above). You can, however, use a [Script Collector](collectors-script#examples) to listen for API Server or Worker Group events.

For recommendations about useful Cribl metrics to monitor, see [Internal Metrics](internal-metrics).

### Controlling Metrics Volume {#volume}

To reduce the volume of metrics sent through Cribl Stream, see options on the [Cribl Internal](sources-cribl-internal#rollup) Source. 

#### Disable Field Metrics {#criblmetrics-override}

To send fewer metrics to the Leader Node, you can specify these metrics types in the blocklist at **Settings** > (**Global Settings**) > **System** > **General Settings** > **Limits** > **Metrics** > **Disable field metrics**.

The default fields to ignore are `host`, `source`, `sourcetype`, `index`, and `project`.
You can remove any of the defaults, or add other fields you do not want to send as metrics.

However, when you enable the **Cribl Internal** Source, Cribl Stream ignores this **Disable field metrics** setting, and full-fidelity data will flow down the Routes.

#### Dropping Metrics

When the number of in-flight requests for sending metrics from Worker to Leader exceeds a limit (1,000 requests),
Workers will stop sending metrics. Dropped metrics information is logged under `channel="clustercomm"`.

You can exclude certain metrics from being dropped due to exceeding limits.
Specify these metrics at **Settings** > **System** > **General Settings** > **Limits** > **Metrics** > **Metrics never-drop list**.

This setting is available only on Worker Nodes.

#### Metrics Garbage Collection

Metrics garbage collection (GC) runs when the total number of stored metrics exceeds the max metrics limit (default 1,000,000).
Metrics are then removed, starting with the oldest ones.
This happens both on Workers and Leaders.

You can define how often to run garbage collection on each Worker Group.
Select **Group Settings** > **System** > **General Settings** > **Limits** > **Metrics** > **Metrics GC period**.
The default is 60 seconds.

#### Metrics Tracking by Worker ID

Typically, all metrics are assigned their own Worker Node ID dimensions so they can be split by worker if needed.

You can define which metrics are **not** assigned a Worker Node ID dimension by adding them to the list at
**Group Settings** > **System** > **General Settings** > **Limits** > **Metrics** > **Metrics worker tracking**.

#### Control Metrics Lag and Disk Usage {#disk}

If one or more Worker Groups has a large number of enabled Sources, and clicking into these Sources does not promptly display their status, a workaround is to prevent Cribl Stream from writing metrics to disk. On the Leader (versions 4.1.2 and later), navigate to **Settings** > **Global Settings** > **System** > **General Settings** > **Limits** > **Storage**, and disable the **Persist metrics** toggle. Then restart the Leader.

If you keep **Persist metrics** enabled, you can use the adjacent **Metrics max disk space** field to control the written metrics' footprint. This threshold defaults to `64 GB`.

#### Cardinality Limit

You can define the cardinality limit or metrics on each Worker Group.
Select **Group Settings** > **System** > **General Settings** > **Limits** > **Metrics** > **Metrics cardinality limit**
The default is 1,000.

Refer to **Monitoring** > **Data** > **Data Fields** to identify the fields that have the highest cardinality.

## Search Internal Logs {#log_search}

Cribl Stream exists because logs are great and wonderful things! Using Cribl Stream's **Monitoring > Logs** page, you can search all Cribl Stream's internal logs at once – from a single location, for both Leader and Worker. This enables you to query across all internal logs for strings of interest. 

The labels on this screenshot highlight the key controls you can use (see the descriptions below):

![Logs page (controls highlighted)](Log-Screen-annotated.c434254758.png)
{border="true"}

1. **Log file selector**: Choose the Node to view. In a distributed deployment ([Stream](/stream/deploy-distributed)), this list will be hierarchical, with Workers displayed inside their Leader.

2. **Fields selector**: Click the **Main | All | None** toggles to quickly select or deselect multiple check boxes below. Beside these toggles, a Copy button enables you to copy field names to the clipboard in CSV format.

   ![Monitoring - Copy Fields Icon](monitoring-copy-fields-icon.278cf34e7f.png)
   {scale="60%" border="true"}

3. **Fields**: Select or deselect these check boxes to determine which columns are displayed in the Results pane at right. (The upper **Main Fields** group will contain data for *every* event; other fields might not display data for all events.) 

4. **Time range selector**: Select a standard or custom range of log data to display. (The **Custom Time Range** pickers use your local time, even though the logs' timestamps are in UTC.) {{< id `time-range-selector` >}}

5. **Search box**: To limit the displayed results, enter a JavaScript expression here. An expression must evaluate to `truthy` to return results. You can press **Shift+Enter** to insert a newline. 

  Typeahead assist is available for expression completion:

  ![Expression autocompletion](log-expression-autocomplete.b64138c5af.png)
  {border="true"}

  Click a field in any event to add it to a query:

  ![Click to add a field](log-click-insert-field.a5ffc1468c.png)
  {border="true"}

  Click other fields to append them to a query:

  ![Click to append a field](log-click-append-field.a414bae939.png)
  {border="true"}

  Shift+click to _negate_ a field:

  ![Shift-click to negate a field](log-shiftclick-exclude-field.754636fd32.png)
  {border="true"}

  > To modify the depth of information that is originally input to the Logs page, see [Logging Settings](#log_settings).
  {.box .info}

6. Click the Search box's history arrow (right side) to retrieve recent queries:

  ![Retrieve recent searches](recent-searches.487558291b.png)
  {border="true"}

7. The Results pane displays most-recent events first. Each event's icon is color-coded to match the event's severity level.

  Click individual log events to unwrap an expanded view of their fields:  

  ![Log event details](log-row-expanded.734f659c87.png)
  {scale="50%" border="true"}

8. **Export Logs as JSON** button: Exports logs as a file initially named `CriblMonitoringLogs.json`. (You can edit this name upon export.) The logs' scope will be filtered both by your [Time range selector](#time-range-selector) setting, and by how fully you've scrolled down to lazy-load that time range into the displayed UI. {{< id `export` >}}

## Logging Settings {#log_settings}

On Cribl Stream's Settings pages, you can adjust the level (verbosity) of internal logging data processed, per logging channel. You can also redact fields in customized ways. In a Distributed Deployment, you manage each of these settings per Worker Group. 

The logging levels help to categorize the detail and severity of logged information. Choosing the right level ensures you get the information you need for troubleshooting without getting overwhelmed by a flood of data.

Here's a breakdown of each logging level:

| Code | Description |
|---|---|
| `error` | Logs only critical issues that have occurred, which need immediate attention to ensure the system's integrity.|
| `warn` | Logs warnings about potential issues that might not be critical but warrant attention to prevent future problems. |
| `info` | Logs general information about the system's operation, providing insights into standard processes and procedures. |
| `debug` | Logs detailed information for diagnosing and troubleshooting potential problems, providing deep insights into the system's behavior. |
| `silly` | Logs extremely detailed information and metrics, primarily used for in-depth troubleshooting of inputs and other operations.Use it sparingly and only for short debugging bursts, treating it as "use at your own risk." Enable temporarily, then disable promptly.|

By default, all the integration logging levels are set to `info`. This default setting displays logs whose level is `info` and more-severe (`warn` and `error`). To expose more-verbose `debug` and `silly` logs, adjust this setting per integration. 

>In Cribl.Cloud, you can't directly change the logging level on Cribl-managed Worker Groups or their integrations. These levels take the default `info` level. To adjust these levels, please contact Cribl Support, who will engage the appropriate team to make the necessary changes for you.
>
{.box .cloud}

### Change Logging Levels {#levels}

To adjust logging levels:

- In a single-instance deployment, select **Settings** > **System > Logging > Levels**. 

- For the Leader Node's own logs, select **Settings** > **Global Settings** > **System > Logging > Levels**. 

- In a Distributed Deployment, first select **Manage**, then click into the group you want to configure. Next, select **Group Settings** (or **Fleet Settings**) > **System > Logging > Levels**. 

On the resulting **Manage Logging Levels** page, you can:

* Modify one channel by clicking its **Level** column. In the resulting drop-down, you can set a verbosity level ranging from **error** up to **debug**. (Top of composite screenshot below.)

* Modify multiple channels by selecting their check boxes, then clicking the **Change log level** drop-down at the bottom of the page. (Bottom of composite screenshot below.) You can select all channels at once by clicking the top check box. You can search for channels at top right.

![Manage Logging Levels page](log-levels-composite.c781d3426b.png)
{border="true"}


### Change Logging Redactions

On the **Redact Internal Log Fields** page, you can customize the redaction of sensitive, verbose, or just ugly data within Cribl Stream's internal logs. To access these settings:

- In a single-instance deployment, select **Settings** > **System > Logging > Redactions**.

- In a single-instance deployment, or for the Leader Node's own logs, select **Settings** > **Global Settings** > **System > Logging > Redactions**.

- In a Distributed Deployment, first select **Manage**, then click into the group you want to configure. Next, select that group's **Group Settings** > **System > Logging > Redactions**. 

![Redact Internal Log Fields page](log-redaction-screen.fe5dea3626.png)
{border="true"}

It's easiest to understand the resulting **Redact Internal Log Fields** page's fields from bottom to top:

* **Default fields**: Cribl Stream always redacts these fields, and you can't modify this list to allow any of them through. However, you can use the two adjacent fields to define stricter redaction:
* **Additional fields**: Type or paste in the names of extra fields you want to redact. Use a tab or hard return to confirm each entry.
* **Custom redact string**: Unless this field is empty, it defines a literal string that will override Cribl Stream's default redaction pattern (explained below) on the affected fields.

#### Default Redact String

By default, Cribl Stream transforms this page's selected fields by applying the following redaction pattern:

* Echo the field value's first two characters.
* Replace all intermediate characters with a literal `...` ellipsis.
* Echo the value's last two characters.

Anything you enter in the **Custom redact string** field will override this default `??...??` pattern.
