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

**Advanced** mode is a Javascript input field where you can create your own filter,
referencing the JSON objects that are associated with an Edge Node.  

You can edit the Javascript by typing in the **Expression** field, but doing so
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
   **Advanced** on to write your own filter using Javascript.
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
has an associated Javascript property. You'll see the Javascript property in the
**Expressions** field when you toggle **Advanced** on.

|Field|Description|Javascript property|
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

You can use the following Node properties and types to create your Advanced filter
in Javascript.

<div data-reactroot="">
<table class="table" style="border-color: black; width: 100%;" border="2">
<thead>
<tr>
<th>Node Property</th>
<th>Type</th>
</tr>
</thead>
<tbody>
<th>
<tr>
<td>id</td>
<td>string</td>
</tr>
<tr>
<td>status</td>
<td>string (Healthy)</td>
</tr>
<tr>
<td>group</td>
<td>string (Fleet name)</td>
</tr>
<tr>
<td>info</td>
<td>
<table style="border-color: black; width: 100%;" border="1">
<tbody>
<tr>
<td>hostname</td>
<td>string&nbsp;</td>
</tr>
<tr>
<td>platform</td>
<td>string (OS Platform;&nbsp;<em>linux, win32 etc.</em>)</td>
</tr>
<tr>
<td>architecture</td>
<td>string (CPU Architecture;&nbsp;<em>arm64, x64 etc.</em>)</td>
</tr>
<tr>
<td>release</td>
<td>string (OS Kernel release)</td>
</tr>
<tr>
<td>cpus</td>
<td>number (Number of CPU cores)</td>
</tr>
<tr>
<td>totalmem</td>
<td>number (Memory in bytes)</td>
</tr>
<tr>
<td>node</td>
<td>string (NodeJS version)</td>
</tr>
<tr>
<td>cribl</td>
<td>
<table style="border-color: black; width: 700px;" border="1">
<tbody>
<tr>
<td>startTime</td>
<td>number (epoch timestamp in msecs when the process started)</td>
</tr>
<tr>
<td>guid</td>
<td>string (unique instance ID)</td>
</tr>
<tr>
<td>installType</td>
<td>string <ul>Install Type. Can be CONTAINER or RPM.<br>If this is set, Leader upgrades are skipped)</td>
</tr>
<tr>
<td>tags</td>
<td>string array (tags configured on the Node)&nbsp;</td>
</tr>
<tr>
<td>version</td>
<td>string (Edge version)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td>freeDiskSpace</td>
<td>number (Free disk space in bytes)</td>
</tr>
<tr>
<td>totalDiskSpace</td>
<td>number (Total disk space in bytes)</td>
</tr>
<tr>
<td>kube (this will only be populated for Nodes running in a Kubernetes Cluster)</td>
<td>
<table class="table table-condensed table-sm" style="border-color: black; width: 848px;" border="1">
<tbody>
<tr>
<td>source</td>
<td>string (always cluster)</td>
</tr>
<tr>
<td>node</td>
<td>string (Node name where this Pod is running)</td>
</tr>
<tr>
<td>pod</td>
<td>string (Pod Name)</td>
</tr>
<tr>
<td>namespace</td>
<td>string (namespace, always cribl)</td>
</tr>
<tr>
<td>owner</td>
<td>
<div>
<table class="table table-condensed table-sm" style="border-color: black; width: 848px;" border="1">
<tbody>
<tr>
<td>kind</td>
<td>string (DaemonSet)</td>
</tr>
<tr>
<td>name</td>
<td>string (cribl-edge)</td>
</tr>
</tbody>
</table>
</div>
</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td>conn ip</td>
<td>string (this is the IP that Cribl Leader sees; different from actual IP of the Node)</td>
</tr>
<tr>
<td>localTime</td>
<td>number (local epoch time on the Node in msec)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td style="width: 137.781px;">lastMsgTime</td>
<td style="width: 706.219px;">number (time when Node sent the last message in msec)</td>
</tr>
<tr>
<td style="width: 137.781px;">firstMsgTime</td>
<td style="width: 706.219px;">number (time when Node sent the first message in msec)</td>
</tr>
<tr>
<td style="width: 137.781px;">nodeUpgradeStatus</td>
<td style="width: 706.219px;">
<table class="table table-condensed table-sm" style="width: 1061px;">
<tbody>
<tr>
<td style="width: 182.594px;">timestamp</td>
<td style="width: 876.406px;">number (time when Node entered the state below in msec)</td>
</tr>
<tr>
<td style="width: 182.594px;">state</td>
<td style="width: 876.406px;">
<p>number</p>
<p>0 for Current (Node is upgraded to version setting in the Fleet)</p>
<p>1 for Active (Node is being upgraded to version setting in the Fleet)</p>
<p>3 for Failed (upgrade failed when tried to upgrade to version setting in the Fleet)</p>
</td>
</tr>
<tr>
<td style="width: 182.594px;">skipped</td>
<td style="width: 876.406px;">number (if set, this Node is skipped by thr Leader's automatic upgrade.&nbsp; See InstallType above)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td style="width: 137.781px;">lastMetrics</td>
<td style="width: 706.219px;">
<table class="table table-condensed table-sm" style="border-color: black; width: 848px;" border="1">
<tbody>
<tr>
<td style="width: 191.25px;">edge.dropped events</td>
<td style="width: 653.75px;">number (events dropped by Node)</td>
</tr>
<tr>
<td style="width: 191.25px;">health.inputs</td>
<td style="width: 653.75px;">
<p>number (Source health status)</p>
<p>0 - All sources in the Node are healthy</p>
<p>1 - One or more sources in the Node have warnings</p>
<p>2 - One or more sources in the Node have errors.</p>
</td>
</tr>
<tr>
<td style="width: 191.25px;">health.outputs</td>
<td style="width: 653.75px;">
<p>number (Destination health status)</p>
<p>0 - All sources in the Node are healthy</p>
<p>1 - One or more sources in the Node have warnings</p>
<p>2 - One or more sources in the Node have errors</p>
</td>
</tr>
<tr>
<td style="width: 191.25px;">total.in_events</td>
<td style="width: 653.75px;">number (total events ingested by Node)</td>
</tr>
<tr>
<td style="width: 191.25px;">total.out_events</td>
<td style="width: 653.75px;">number (total events sent out by Node)</td>
</tr>
<tr>
<td style="width: 191.25px;">total.dropped_events</td>
<td style="width: 653.75px;">number (total events dropped by Node)</td>
</tr>
<tr>
<td style="width: 191.25px;">total.in_bytes</td>
<td style="width: 653.75px;">number (total ingested bytes)</td>
</tr>
<tr>
<td style="width: 191.25px;">total.out_bytes</td>
<td style="width: 653.75px;">number (total bytes egressed)</td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
</div>

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