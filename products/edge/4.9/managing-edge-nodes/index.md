# Manage Edge Nodes


How to manage Edge Nodes in a Fleet and perform tasks like teleporting and
exporting a list of Nodes

---

Accessing the **Edge Nodes** page is available on certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).

## View Edge Node Status Information

The **Edge Nodes** page provides status information for each Edge Node in the
selected Fleet, as well as a UI for adding and updating Nodes.

In Cribl Edge, select **Edge Nodes** from the sidebar.

![Edge Nodes page showing a list of Nodes in the selected Fleet](edge-nodes-page.png)
{border="true" scale="75%"}

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

> In Cribl.Cloud, you must accept the confirmation modal. Changing the tags on
> an Edge Node can affect the Fleet that the Leader maps it into.
{.box .cloud}