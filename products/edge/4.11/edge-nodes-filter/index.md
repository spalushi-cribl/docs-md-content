# Filter Edge Nodes


Learn how to filter large lists of Edge Nodes

---

When your Cribl Edge deployment has a large number of Nodes, you may want to
filter your Node list down to a manageable subset of Nodes. This can help you
monitor your Edge Nodes and troubleshoot any issues. 

For example, you may want to view all of the Edge Nodes that are experiencing
upgrade issues. Or, you may need to update the configuration of Nodes that are
running a certain operating system version, but you don't know which Fleets they
belong to.

The **Filters** option allows you to apply filter criteria to your Node list,
so you can quickly find Nodes by OS Platform, IP address, install type, version
and more.

## Access Node Filters

In Cribl Edge, you can filter Edge Nodes in two places:

- In a specific Fleet in the Edge Nodes list. Navigate to **Fleets**, then select a Fleet.
- On the list of all Edge Nodes, in both List View and Map View. Navigate to
  **Edge Nodes** from the sidebar.

![The Filters option](filters-ui.png)
{border="true" scale="70%"}

### Filter Modes

You can choose between the default form-based mode and **Advanced** mode when you create a
Node filter. Form-based mode contains the preconfigured
fields and checkboxes you see when you select **Filters**. 

For a table of the options and their
definitions, see [Form-based Filter Options](#node-filter-options).

**Advanced** mode is a JavaScript input field where you can create your own filter,
referencing the JSON objects that are associated with an Edge Node.  

You can edit the JavaScript by typing in the **Expression** field, but doing so
disables the form-based filters unless you undo the changes. To revert
back to the form-based filter, select **Undo changes**. 

> Some filter combinations will never return any results – for example, **OS
> platform** set to Linux and **Install type** set to MSI, since MSI is only used for
> Windows installations.
{.box .info}

## Create an Edge Node Filter

The **Filters** option lets you group a list of Nodes by criteria
such as OS platform, IP address, install type, version, and more.

To create an Edge Node filter:

1. Navigate to a list of Edge Nodes, either from the **Nodes** tab in a Fleet or
   from the **Edge Nodes** sidebar.
1. Select **Filters** to open the Filters panel.
1. Create a filter by selecting from the [Node Filter Options](#node-filter-options), or toggle
   **Advanced** on to write your own filter using JavaScript.
1. Once you have selected filter criteria, you can use the **Filter Edge Nodes**
   field to further narrow down the list of visible Nodes.
1. Select **Clear filters** to undo your selections in the Filters panel. This
   does not clear the **Filter Edge Nodes** field.

### Share a Filter

To share a filter with another Cribl user:

1. Select the **Filters** button and create a filter. 
1. Select the link icon ![](link-icon.png) to copy the
   filters to your clipboard, or copy the browser URL. 
1. Now you can share the link with another user.

Filters are Workspace-specific, so they will only work within the Workspace
where you created it.

### Form-based Filter Options

You can choose from a set of preconfigured, form-based filter options, and each
has an associated JavaScript property. You'll see the JavaScript property in the
**Expressions** field when you toggle **Advanced** on.

|Field|Description|JavaScript property|
|-----|-----------|-------------------|
|<div style="width: 125px">Fleet</div>|Find Nodes based on their Fleet assignment. Visible when filtering from the **Edge Nodes** page.|`group=='<fleet>'`|
|Upgrade status|The upgrade status of the Edge Node.<ul><li><strong>Current</strong>: When the Fleet has a [Target Version](upgrading-ui-status#view-the-status-of-fleet-upgrades) set, this filters Nodes that are running the same version as the Fleet or a newer version.</li><li><strong>Active</strong>: Nodes that are actively upgrading.</li><li><strong>Skipped</strong>: Nodes with a skipped upgrade.</li><li><strong>Failed</strong>: Nodes that failed to upgrade.|`nodeUpgradeStatus.state`|
|Source status|The health status for each Source associated with the Nodes.|`(lastMetrics['health.inputs'])`|
|Destination status|The health status for each Destination associated with the Nodes.|`(lastMetrics['health.outputs'])`|
|Edge version|Enter a semantic version of Cribl Edge. For example, 4.1.0.|`info.cribl.version.toLowerCase()`|
|Host|The host of the Edge Node. You can type in any part of the host string.|`info.hostname.toLowerCase().includes(<host>)`|
|OS platform|The operating system platform that the Edge Node is running on.|`(info.platform === '<os-platform>')`|
|OS version|The version of the operating system the Edge Node is running on. For example, Windows 10.|`info.release.toLowerCase().includes('<OS version>')`|
|Architecture|The processor architecture for the Edge Node.|`(info.architecture === '<architecture>')`|
|IP address|The IP address of the Edge Node. Entering a partial value returns all Nodes with matching values. For example, entering `17` will return all Nodes that have `17` somewhere in the address.|`info.conn_ip.toLowerCase().includes('<ip-here>')`| 
|Install type|The installation type for the Node. Select RPM or Container. Useful for determining which Nodes can't be upgraded by the Leader.|`(info.env.CRIBL_INSTALL_TYPE === '<type>')`|
### Advanced Filter Options

You can use JavaScript expressions in the Advanced filter to surface more specific, granular search results.
You can use any Edge Node properties and types in the **Expression** field.

The JavaScript expression should be in this format, in hierarchical order from
parent to child: 

`parent.child.grandchild === 'value'`

For example: 

`info.aws.type === 't2.medium'`.  

You can also automatically populate the Advanced filter **Expression** field:

1. Select the **JSON view** button.
1. Expand the Node row and select the Node property, then any applicable child properties.
1. Type directly in the **Expression** field to fine-tune the filter.

   ![Filter Edge Nodes in JSON view](filter-nodes-advanced-data.png)
   {border="true" scale="75%"}

#### Supported Node Properties

You can filter on these Node properties using a JavaScript expression. Remember
to separate the parent and child properties using a period `.`.

```
  "id": "string",
  "status": "string (Healthy)",
  "group": "string (Fleet name)",
  "info": {
    "hostname": "string",
    "platform": "string (OS Platform; linux, win32 etc.)",
    "architecture": "string (CPU Architecture; arm64, x64 etc.)",
    "release": "string (OS Kernel release)",
    "cpus": "number (Number of CPU cores)",
    "totalmem": "number (Memory in bytes)",
    "node": "string (NodeJS version)",
    "cribl": {
      "startTime": "number (epoch timestamp in msecs when the process started)",
      "guid": "string (unique instance ID)",
      "installType": "string",
      "tags": "string array (tags configured on the Node)",
      "version": "string (Edge version)"
    },
    "freeDiskSpace": "number (Free disk space in bytes)",
    "totalDiskSpace": "number (Total disk space in bytes)",
    "kube": {
      "source": "string (always cluster)",
      "node": "string (Node name where this Pod is running)",
      "pod": "string (Pod Name)",
      "namespace": "string (namespace, always cribl)",
      "owner": {
        "kind": "string (DaemonSet)",
        "name": "string (cribl-edge)"
      }
    "conn ip": "string (this is the IP that Cribl Leader sees; different from actual IP of the Node)",
    "localTime": "number (local epoch time on the Node in msec)"
  "lastMsgTime": "number (time when Node sent the last message in msec)",
  "firstMsgTime": "number (time when Node sent the first message in msec)",
  "nodeUpgradeStatus": {
    "timestamp": "number (time when Node entered the state below in msec)",
    "state": "number",
    "skipped": "number (if set, this Node is skipped by thr Leader's automatic upgrade. See InstallType above)"
  "lastMetrics":
    "edge.dropped events": "number (events dropped by Node)",
    "health.inputs": "number",
    "health.outputs": "number",
    "total.in_events": "number (total events ingested by Node)",
    "total.out_events": "number (total events sent out by Node)",
    "total.dropped_events": "number (total events dropped by Node)",
    "total.in_bytes": "number (total ingested bytes)",
    "total.out_bytes": "number (total bytes egressed)"
```

### Example Advanced Filter Expressions

You can use these example expressions as a starting point to build your advanced
filters.

**Edge Nodes with >10 CPUs and >8 GB RAM**

Because total memory is reported in bytes, we need to
calculate 8GB to bytes before using it in the filter.

```JS
info.cpus >= 4 && info.totalmem >= 8388608000
```

**Nodes running the System State Source**

```JS
lastMetrics.hasOwnProperty('health.inputs:input:system_state:in_system_state')
```

**Nodes Running the System State Source and experiencing issues with Destinations**

```JS
lastMetrics.hasOwnProperty('health.inputs:input:system_state:in_system_state') && lastMetrics['health.outputs'] === 2
```

**Nodes with errors in either Sources or Destinations**

```JS
lastMetrics['health.inputs'] === 2 || lastMetrics['health.outputs'] === 2
```