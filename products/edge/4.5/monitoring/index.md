# Monitoring


# Monitoring Health and Metrics

To get an operational view of your Cribl Edge deployment, consult the following resources.

### Landing Pages

When you first log into your distributed deployment, you will be greeted with tiles prompting you to choose a product. The Cribl Edge tile displays basic configuration details, including number of Fleets, Subfleets, Edge Nodes, and events and bytes over time.

![Manage Edge Tile](managed_edge_landing.4f11c1cdf5.png)
{border="true"}

Select **Manage** to navigate to the Cribl Edge **Home** page, which shows aggregate data for all Fleets, Subfleets and Edge Nodes. The charts display information about traffic in and out of the system. 

On top of the **Bytes** chart, you can change the display's granularity from the default `last 5 min`, selecting a variety of time ranges from `1 min` up to `1 day` (covering the preceding 24 hours).  

![Cribl Edge Home page](ed-edge_home.5e781321cd.png)
{border="true"}

Select **Manage** from the top nav to view the **Fleets Landing** page. Here you can access the tabs for more information about your **Fleets** (and **Subfleets**), **Edge Nodes**, **Mappings**, [**Notifications**](notifications), and **Logs**.

![Manage Fleets](ed-fleet_all.eceaf7be87.png)
{border="true"}

The **Manage Fleets** page gives you access to more information about your **Fleets** (and **Subfleets**), **Edge Nodes**, **Mappings**, [**Notifications**](notifications), and **Logs**.

You can click a **Fleet** link to isolate individual Fleets, or use the **Search** bar to locate your Fleet. 

![Fleet Landing page](ed_edge_fleet_landing.6d95cbcc21.png)
{border="true"}

### Source and Destination Health Status

When you add or manage configured Sources and Destinations, you’ll see a colored health status (if [enabled](#enabling-health-status)). In the list view, this status is a green, orange, red, or gray dot next to the name, along with a gray circle indicating the number of instances you have configured. 

![Source list view](edge-health-list.b16d9533fe.png)
{scale="30%"}

In the tile view, colors are applied to the squares that indicate the number of instances you have configured.

![Source tile view](edge-health-tiles.bde2f8f1fc.png)
{scale="75%"}

The colors have the following meanings for each Source and Destination:
* **Green**: Working properly
* **Orange**: Experiencing problems
* **Red**: Experiencing severe problems
* **Gray**: Not enabled

To learn more about the health status of any Source or Destination, click its name to open the list of existing Sources or Destinations, and then click the Status icon for the one you want to examine. 

![Health status icon](edge-health-status-icon.729d84b19d.png)

In the modal that opens, click the Status icon again to see more details. 

![Health status details](edge-health-status-details.c343272d59.png)

When there are more than 100 Nodes in the Fleet, the **Status** column will not
be visible in the list of Nodes. To view the status for an individual Node,
expand it in the list. You can only expand one Node at a time. Refresh the list
of Nodes and their status with the **Refresh** button. 

#### Enabling Health Status

Health status must be enabled independently for each fleet in Fleet Settings. To enable health status for a Fleet:
1. Select the Fleet you want to manage.
2. On the **Manage** page, select the **Fleet Settings** tab. 
3. Navigate to **General Settings** > **Limits** > **Metrics**.
4. Under **Metrics to send from Edge Nodes**, use the buttons to select or create a set of metrics.
> If **Minimal** is selected, health status will not be enabled for the Fleet and health icons will appear white for all Sources and Destinations. 
>
{.box .info}

To learn more about these metrics options, see [Controlling Metrics](internal-metrics#controlling-metrics).

### Health Status

You can isolate throughput for any of the following:

- Sources
- Destinations
- Routes
- Pipelines
- Packs
- Data Fields (Edge Nodes only)

For Fleets, go to the **Health** tab and click the tab you want. For Edge Nodes, go to the Edge Node's **Health** tab, and then select an option on the **Data** submenu. 

![Manage > Health tab](edge-health-tab.ecf456a6c2.png)
{border="true"}

Dense displays are condensed to sparklines for legibility. 

The **Status** column displays the health of each resource with a colored icon. Click the icon to see more details. If a Fleet’s **Health** tab is empty, health status might not be enabled because only minimal metrics are being sent. You can enable health status by going to **Fleet Settings** > **General Settings** > **Metrics** and selecting **Basic** or **All** under **Metrics to send from Edge Nodes**. See [Controlling Metrics](internal-metrics#controlling-metrics) to learn more about these options. 

Hover over the right edge to display Maximize buttons that you can click to zoom these up to detailed graphs.



You can hover over an expanded graph fly-out to display further details.

![Throughput details](ed_monitoring_details.f4ac97b4be.png)
{border="true"}

### Edge Node Health 

To review the health status of an Edge Node, teleport into the Node. From the **Fleet** landing page, click the **List View** tab, and then click the **Edge Node GUID** link to teleport from the Leader into the Edge Node. From the top nav of the Edge Node, click the **Health** tab for monitoring details. 

![Health Overview](ed_edge_teleport_health.ea83e81634.png)

> Metrics are displayed in the Cribl Edge UI in units of KB, MB, GB, and TB. At each level, these are multiples of `1024`.
>
{.box .info}

The displayed **Storage** represents the amount of free storage remaining on the partition where Cribl Stream is installed. (This quantity might not represent the maximum storage available for the selected Edge Node or Fleet. Also, it does not calculate the system free space.)

Similarly, the **Free Memory** graph reflects only the operating system's `free` statistic, matching Linux's strict `free` definition by excluding `buff/cache` memory. So this graph indicates a lower value than the OS' `available` memory statistic – and it does not necessarily indicate that the OS is running out of memory to allocate.

Byte-related charts show the uncompressed amounts and rates of data processed over the selected time range: 
- Events (total) in and out
- Events per second in and out
- Bytes (total) in and out
- Bytes per second in and out

We measure bytes in and out based on the size of `_raw`, if this field is present and is of type `string`. Otherwise, we use the size of read (uncompressed) data.  

The displayed **CPU Load Average** is an average Edge Node Process, updated at 1‑minute granularity. (It is not an average for the Edge Node as a whole.)

> Monitoring data does not persist across Cribl Edge restarts. Keep this in mind before you restart the server.
>
{.box .warning}

#### System Monitoring

From the **Health** page's top nav, open the **System** submenu to isolate throughput for:

- Queues (Sources) 
- Queues (Destinations)

For details, see [Persistent Queues](persistent-queues).

#### Reports/Top Talkers 

Select **Health** > **Reports / Top Talkers** > **Top Talkers**, where you can examine your five highest-volume Sources, Destinations, Pipelines, Routes, and Packs. All components are ranked by events throughput. Sources and Destinations get separate rankings by bytes in and out, respectively.

![Top Talkers](ed_top_talkers.35f796bc22.png)
{border="true"}

#### Internal Logs and Metrics

Select **Logs** from the **Health** page's top nav. Cribl Edge's [internal logs](#types) and [internal metrics](internal-metrics) provide comprehensive information about an instance's status/health, inputs, outputs, Pipelines, Routes, Functions, and traffic.

### Health Endpoint {#endpoint}

Each Cribl Edge instance exposes a [`/health` endpoint](/api/query-health-endpoint) that is commonly used along with a Load Balancer to support operational decision-making. See [Leader High Availability/Failover: Load Balancers](deploy-add-second-leader#loadbalancers) for more information.

> For many HTTP-based [Sources](sources), you can enable a Source-level health check endpoint in the **Advanced Settings** tab. The request URL format for these endpoints is `http(s)://${hostName}:${port}/cribl_health`. Refer to the configuration instructions for a Source to learn whether the health check endpoint is available for it. 
>
{.box .info}

##  Types of Logs  {#types}

Cribl Edge provides the following log types, by originating process:

* API Server Logs – These logs are emitted primarily by the `API/main` process. They correspond to the top-level `cribl.log` that shows up on the [Diag](/stream/diagnosing) page. These include telemetry/license-validation logs. Filesystem location: `$CRIBL_HOME/log/cribl.log`

* Edge Node Process(es) Logs – These logs are emitted by all the Edge Node Processes, and are very common on single-instance deployments and Edge Nodes. Filesystem location: `$CRIBL_HOME/log/worker/N/cribl.log`

* Fleet Logs – These logs are emitted by all processes that help a Leader Node configure Fleets. Filesystem location: `$CRIBL_HOME/log/group/GROUPNAME/cribl.log`

> For details about generated log files, see [Internal Logs](internal-logs). To work with logs in Cribl Edge's UI, see [Search Internal Logs](#log_search).
>
{.box .success}



## Log Rotation and Retention {#retention}

Cribl Edge rotates logs every 5 MB, keeping the most recent 5 logs. Logs will reach the 5 MB threshold more quickly with verbose [logging settings](##log_settings) (such as `debug`) and with high volumes of system activity. In a
Distributed deployment ([Edge](setting-up-leader-and-edge-nodes)), all Edge Nodes
forward their metrics to the Leader Node, which then consolidates them to
provide a deployment-wide view.

For long-term retention, Cribl recommends sending logs from the Leader, and the [Cribl Internal](sources-cribl-internal#configuring) > CriblLogs Sources on Edge Nodes, to a downstream service of your choice. See the next section for details.

## Forward Logs and Metrics Externally {#external-monitoring}

Cribl Edge supports forwarding internal logs and metrics to your preferred external monitoring solution. To make internal data available to send out, go to **More** > **Sources** and enable the [Cribl Internal](sources-cribl-internal) Source. 

This will send internal logs and metrics down through [Routes](routes) and [Pipelines](pipelines), just like another data source. Both logs and metrics will have a field called `source`, set to the value `cribl`, which you can use in Routes' filters. 

Note that the only logs supported here are Edge Node  Process logs (see [Types of Logs](#types) above). You can, however, use a [Script Collector](/stream/collectors-script#examples) to listen for API Server or Edge Node Fleet events.

For recommendations about useful Cribl metrics to monitor, see [Internal Metrics](internal-metrics). 

> ##### CriblMetrics Override
>
> The **Disable field metrics** setting – in  **Settings** (top nav) > **System > General Settings > Limits** ‑ applies only to metrics sent to the Leader Node. When the **Cribl Internal** Source is enabled, Cribl Edge ignores this **Disable field metrics** setting, and full-fidelity data will flow down the Routes.
>
{.box .info}

## Search Internal Logs {#log_search}

Cribl Edge exists because logs are great and wonderful things! Using Cribl Edge's **Health > Logs** page, you can search all Cribl Edge's internal logs at once – from a single location, for both Leader and Edge Node. This enables you to query across all internal logs for strings of interest. 

The labels on this screenshot highlight the key controls you can use (see the descriptions below):

![Logs page (controls highlighted)](ed_log_screen_annotated.baae8b4c6a.png)
{border="true"}

1. **Log file selector**: Choose the types of logs to view. 

2. **Fields selector**: Click the **Main | All | None** toggles to quickly select or deselect multiple check boxes below. Beside these toggles, a Copy button enables you to copy field names to the clipboard in CSV format.

3. **Fields**: Select or deselect these check boxes to determine which columns are displayed in the Results pane at right. (The upper **Main Fields** group will contain data for *every* event; other fields might not display data for all events.) 

4. **Time range selector**: Select a standard or custom range of log data to display. (The **Custom Time Range** pickers use your local time, even though the logs' timestamps are in UTC.)

5. **Search box**: To limit the displayed results, enter a JavaScript expression here. An expression must evaluate to `truthy` to return results. You can press **Shift+Enter** to insert a newline. 

  > To modify the depth of information that is originally input to the Logs page, see [Logging Settings](#log_settings).
  {.box .info}

6. Click the Search box's history arrow (right side) to retrieve recent queries.

7. The Results pane displays most-recent events first. Each event's icon is color-coded to match the event's severity level.

  Click individual log events to unwrap an expanded view of their fields.  

## Logging Settings {#log_settings}

On Cribl Edge's Settings pages, you can adjust the level (verbosity) of internal logging data processed, per logging channel. You can also redact fields in customized ways. In a distributed deployment, you manage each of these settings on the Edge Node, Fleet Settings, or main Settings for all Fleets. 

### Change Logging Levels {#levels}

To adjust logging levels:

- From Cribl Edge's top nav, select **Settings**   > **System > Logging > Levels**.  

- To configure log levels on a Fleet-level, click **Manage**, then select the Fleet you want to configure. Next, select **Fleet Settings** > **System > Logging > Levels**. 

- To adjust settings on a specific Edge Node, teleport into the Node, then select **Node Settings**   > **System > Logging > Levels**.

On the resulting **Manage Logging Levels** page, you can:

* Modify one channel by clicking its **Level** column. In the resulting drop-down, you can set a verbosity level ranging from **error** up to **debug**. (Top of composite screenshot below.)

* Modify multiple channels by selecting their check boxes, then clicking the **Change log level** drop-down at the bottom of the page. (Bottom of composite screenshot below.) You can select all channels at once by clicking the top check box. You can search for channels at top right.

![Manage Logging Levels page](ed_log-levels-composite.5887f498e3.png)
{border="true"}

> The `silly` (ultra-verbose) logging level is available for all channels. However, it provides additional metrics information only for inputs.
>
{.box .info}

### Change Logging Redactions

On the **Redact Internal Log Fields** page, you can customize the redaction of sensitive, verbose, or just ugly data within Cribl Edge's internal logs. To access these settings:

- In a single-instance deployment, or for the Leader Node's own logs, select  **Settings** (top nav) > **System > Logging > Redactions**.

- In a distributed deployment, first click **Manage**, then select the Fleet you want to configure. Next, select **Fleet Settings** > **System > Logging > Redactions**. 

 ![Redact Internal Log Fields page](ed_log_redaction_screen.4fd1e02da3.png)
 {border="true"}

It's easiest to understand the resulting **Redact Internal Log Fields** page's fields from bottom to top:

* **Default fields**: Cribl Edge always redacts these fields, and you can't modify this list to allow any of them through. However, you can use the two adjacent fields to define stricter redaction:
* **Additional fields**: Type or paste in the names of extra fields you want to redact. Use a tab or hard return to confirm each entry.
* **Custom redact string**: Unless this field is empty, it defines a literal string that will override Cribl Edge's default redaction pattern (explained below) on the affected fields.

#### Default Redact String

By default, Cribl Edge transforms this page's selected fields by applying the following redaction pattern:

* Echo the field value's first two characters.
* Replace all intermediate characters with a literal `...` ellipsis.
* Echo the value's last two characters.

Anything you enter in the **Custom redact string** field will override this default `??...??` pattern.
