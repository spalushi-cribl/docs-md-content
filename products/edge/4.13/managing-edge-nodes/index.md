# Manage Edge Nodes


How to manage Edge Nodes in a Fleet and perform tasks like teleporting and
exporting a list of Nodes.

---



## View Edge Node Status Information

The **Edge Nodes** page provides status information for each Edge Node in the
selected Fleet, as well as a UI for adding and updating Nodes.

In Cribl Edge, select **Edge Nodes** from the sidebar.

![Edge Nodes page showing a list of Nodes](edge-nodes-page-4.13.png)
{border="true" scale="75%"}

### Monitor Edge Node Connectivity

You can track which Edge Nodes are connected to the Leader Node in the **Edge Nodes** page.
Disconnected Edge Nodes show an icon in the **Connection** column
and will have information about the time the Leader last received their heartbeat.

By default, Cribl Edge will retain information about disconnected Nodes between restarts
in a Cribl.Cloud deployment, but not in an on-prem deployment.
To change this behavior on-prem, toggle **Persist Nodes** on in **Global Settings > System > General Settings > Limits > Other**.
This will cause Node tracking to persist across restarts
and will also affect Cribl Stream Workers.

> Toggling **Persist Nodes** will restart the Leader Node.
> 
> When you enable Node tracking, increase the available disk space on the Leader
> relative to the number of Nodes you are tracking and additional metadata.
> Deployments of 250,000 Nodes may require additional 5 GB of space.
{.box .warning}

You can configure how long the Leader should keep track of disconnected Nodes
by configuring the **Time to keep disconnected Nodes** Fleet setting (**General Settings > Fleet Configuration**).
By default the Leader retains information about disconnected Nodes for one day.
You can set it to any value between 0 (which means Nodes are removed within a few minutes) and 90 days.

For Fleets with Edge Nodes that frequently restart with a new GUID, such as those in a Kubernetes environment,
Cribl recommends setting the **Time to keep disconnected Nodes** to 0.

> The **Time to keep disconnected Nodes** setting is not inherited by Subfleets.
{.box .success}

#### Monitor Connectivity of Older Edge Nodes

A Leader Node that is running version 4.13 or newer can monitor Edge Nodes that are using an older version.
However, Cribl always recommends upgrading your Edge Nodes to the newest available version.

### Monitor Source and Destination Health on Edge Nodes

You can monitor and track the health of Sources and Destinations associated with
an Edge Node on the **Edge Nodes** page. This is useful when you have a large
number of Nodes, and want to quickly identify and troubleshoot issues with
Sources and Destinations.

To open the Node Info drawer and view specific monitoring
information, select a health icon. For more information, see [Track Data Activity for an Edge Node](node-data-activity).

### Filter Nodes in the List

You can filter the Edge Nodes that appear in the list using the **Filters** option. For
details, see [Filter Edge Nodes](edge-nodes-filter).

[Snippet not found: content/edge/4.13/snippets/_identify-docker-nodes.md]

### Export a List of Edge Nodes

To export the list of Edge Nodes, click the **Export list as** drop-down.
Select `JSON` or `CSV` as the export file format. If you filter the list
before exporting, the exported list will maintain that filter. You can export the
entire list by removing all filters.

### Teleport into an Edge Node

Click the **Edge Node GUID** link to teleport from the Leader into the
Edge Node. Here, you can explore the metrics and log data that the Node has
discovered automatically, and can manually discover and explore other data of
interest. 

You can use the discovered data to perform root-cause analysis, to troubleshoot,
and to restart the host. If you want to monitor and troubleshoot the Node data
without teleporting, check out the [Node Info Panel](node-data-activity).

The page displays metadata for the Node, and below it, graphs of system
activity. A magenta border indicates you are remotely viewing a host, and
identifies the host's name. 

Click **Restart Edge** to restart the Node. To return to the **Edge Nodes** page, click the **X** close button on the upper right.

![Teleporting into an Edge Node](ed-teleport-node.4cf3e55509.png)
{border="true"}

> Changes that you make on an Edge Node will not propagate to the Leader. Also,
> the Leader will override any changes that you make directly on a Node.
{.box .warning}

#### Add Tags to Edge Nodes in Teleport View

You can add tags to an Edge Node you've teleported into. Tags help the Leader map
an Edge Node to a Fleet. Note that changing the tags on a Node can cause
the Leader to map the Node into a new Fleet.

1. Teleport into an Edge Node.
1. Navigate to **Node Settings** then **Distributed Settings**.
1. Enter tags for this Node and select **Save**.

If your Edge Nodes are managed by Cribl.Cloud, you'll need to accept the confirmation modal. Keep in mind that changing the tags on an Edge Node might cause it to be assigned to a different Fleet by the Leader.

> You can't change tags for Edge Nodes set up using the `CRIBL_DIST_MASTER_URL` environment variable (instead of the `instance.yml` 
> file). This is because these types of Edge Nodes are typically temporary, so any tag changes would not be saved when the Edge Node restarts. If you need tags that persist, configure your Edge Nodes using the `instance.yml` file.
{.box .warning}