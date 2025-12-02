# Tracking Data Activity for an Edge Node


Monitoring and troubleshooting your Edge Nodes guarantees the integrity and
reliability of the data you're collecting from your Nodes. 

Monitoring helps you
detect failures early and take quick corrective actions. Troubleshooting ensures
that any problems are not only fixed but also analyzed for root causes to
prevent further occurrences.

Cribl Edge provides you with Edge Node data activity, so you can monitor and
troubleshoot any issues. You can use this data activity from the Edge Node to find
out if any of the associated Sources, Destinations, Routes, Pipelines, or Packs
are having issues. Then, drill down into individual elements to identify the
potential issues. You can see all of this information without teleporting into
the Edge Node.

## Access Additional Node Information in the Node Info Panel

The Edge Node information and activity drawer gives you more information about
a specific Node. To display additional details and controls, click each row. The
**Node Info** panel will open.

![Node info panel](node-info-panel-gif.gif)

> With **Teleport to Nodes** enabled in Fleet Settings, the GUID becomes a link.
> To open the node information panel in this case, click anywhere in the row
> except the GUID link. 
{.box .success}

### View the System Activity for a Node

You can monitor system metrics (like CPU, memory, disk usage, and more) 
for a specific Edge Node using the **System Activity** tab.

> Before you can view system metrics for a Node, 
> you must enable the [System Metrics Source](sources-system-metrics)
> or [Windows Metrics Source](sources-windows-metrics) for your Fleet. 
{.box .warning}

Once system data is flowing from one of the Metrics Sources, 
select the row for an Edge Node to view its metrics.

![Node info panel system activity](node-info-panel-system-activity.png)

The **System Activity** tab
displays the following system information:

|Field|Description|
|-----|-----------|
|CPUs | Total number of CPUs for this Node, processes, threads, percentages of Sys, User, and Idle, and load average over 1, 5, and 15 minutes.|
|CPU percentage| A graph displaying the total available CPU and CPU usage, in two-minute increments.|
|Memory| Total memory used by this Node, as well as free, used, cached, and swap used/total memory.|
|Used memory| A graph displaying the used memory over two-minute increments.|
|Network| Average and total network data sent and received, and packets sent and received.|
|Network bytes and packets| A graph displaying network bytes and packets sent and received by the Node, in two-minute increments.|
|Disk| Average and total read and write operations and read and write bytes.|
|Disk bytes and operations|A graph displaying disk bytes and operations in two-minute increments.|


### View the Data Activity for a Node

To view data activity, click the row for an Edge Node. 

![Node info panel data activity](node-info-data-activity.png)

The **Data Activity** tab displays the following data information:

|Field|Description|
|-----|-----------|
|Events In and Out |A graph displaying event throughput in (avg), events in, throughput out (avg) and events out.|
|Bytes In and Out|A graph displaying bytes throughput in (avg), bytes in, throughput out (avg) and bytes out.|
|Sources| Each Source that this Node is receiving events from, along with: <ul><li>The Source name, average throughput (events per second), total events, average byte throughput (bytes per second), and total bytes.</li><li>The Source health status, which is useful for identifying Nodes experiencing Source issues and for diagnosing issues with Sources on specific Edge Nodes.</li></ul>|
|Destination| Each Destination that this Node is sending events to, along with: <ul><li>The Destination name, average throughput (events per second), total events, average byte throughput (bytes per second), and total bytes.</li><li>The Destination health status, which is useful for identifying Nodes experiencing Destination issues and for diagnosing issues with Destinations on specific Edge Nodes.</li></ul>|
|Routes|The Routes configured on this Node, along with average throughput (events per second and bytes per second), total events, and total bytes.|
|Pipelines|The Pipelines this Node sends events through, along with average throughput (events per second in and out) and total events in and out.|
|Packs|The Packs associated with this Node, along with average throughput (events per second in and out) and total events in and out.|

The health status icons indicate the overall health of the Sources, Destinations, Routes, Pipelines, and Packs:

- ![](icon-healthy.png) Green checkmark: Everything is good! All resources are healthy.
- ![](icon-warning.png)Yellow warning icon: Attention needed. One or more resources are in a warning state.
- ![](icon-danger.png)Red exclamation point: Critical issue! One or more resources have encountered an error.
- ![](icon-no-data.png)Indeterminate: No data available yet.

### View Additional Edge Node Info

To view detailed information, click the row for an Edge Node. 

![Node info panel data activity](node-info.png)

The **Node Info** tab displays the following Node information:

|Field|Description|
|-----|-----------|
|Node info |Detailed information for this Node: <ul><li>GUID</li><li>CPUs</li><li>Host</li><li>RAM</li><li>IP Address</li><li>Last heartbeat</li><li>Health</li><li>Date started</li><li>Source health</li><li>Destination health</li><li>Fleet</li><li>Config version</li><li>Edge version</li><li>Messages</li></ul>.|
|Key:Value pairs|When an Edge Node first connects to the Leader, it sends an initial report, which is the data shown here. This report includes specifics about the operating system, the underlying infrastructure (such as Kubernetes or EC2), and the Cribl configuration settings.|
