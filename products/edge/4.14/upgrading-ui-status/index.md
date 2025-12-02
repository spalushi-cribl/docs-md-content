# Monitoring Node Upgrade Status


Learn how to monitor the upgrading status via the UI.

---

To view the upgrade status of eligible Edge Nodes in a Fleet, select the **Target version** button.

You can find this button on the **Fleets** page (**Manage > Fleets**) or in the
top bar of a selected Fleet.

![Target version button on **Manage > Fleets**](target-software-version-column.png)
{scale="70%" border="true"}

![Target version button at the top of a selected Fleet](target-version-button.png)
{scale="70%" border="true"}

When you select this button, the **Target Version** window will open.

### Explore the Target Version Window

The **Target Version** window provides information about Edge Node upgrades
and allows you to select an upgrade target version for your Edge Fleets.

![A popup window titled "Target Version" that shows the upgrade status of Edge Nodes in a Fleet](fleet-upgrade-status.png)
{scale="90%" border="true"}

### View the Status of Fleet Upgrades

The **Upgrade status** tab shows an overview of the Edge Nodes in this Fleet and
the upgrade status for each Node version. The Nodes are organized by version,
and the status columns display how many Nodes are in each status.

In this tab, you can view:

|Status|Description|
|------|-----------|
|<div style="width: 150px">Version</div>|Version for the Edge Nodes in this Fleet.|
|Total Nodes|Total number of Nodes in this Fleet running this version.|
|Upgrading|Nodes that are actively upgrading.|
|Skipped|Number of Nodes that were not upgraded due to one of these reasons: <ul><li>The Fleet [target version](upgrade-nodes-fleet#upgrade-edge-nodes) is not set.</li><li>No suitable upgrade package was found. You should use the [**Path** package upgrade](upgrade-nodes-fleet#package-sources) to add the specific upgrade package.</li><li>The Edge Node version is too old to receive an upgrade:<ul><li>Version 4.3.0 and older for Windows Nodes</li><li>CentOS 6.x Nodes</li></ul></li><li>The Leader couldn't download the upgrade package from the CDN.</li></ul>
|Failed|The Node was not upgraded due to an error. The Node will retry the upgrade in the next hour.|

### View Upgrades for Legacy Edge Nodes 

The **Legacy Upgrade Jobs** tab shows Leader-scheduled jobs that will upgrade
Edge Nodes running versions 4.4.4 or older. If there are no legacy upgrade jobs,
this tab will not show.

In this view, you can view the status the upgrade job per Edge Node. 

- You can use the **Actions** icons to perform actions on the upgrade job:
  pause, stop, copy the link, delete, and view logs. If you cancel a job, you can
  view its artifacts.
- Click the **ID** to view job stats, settings, a live view, and logs.

