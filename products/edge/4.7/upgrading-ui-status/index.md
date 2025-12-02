# Upgrading Status


Learn how to monitor the upgrading status via the UI.

---

## Fleet Upgrade Information

You can use the **Upgrade status** icon from the **List View** of a Fleet to open the **Fleet Upgrade Information window**. This shows an overview of upgrade-eligible Edge Nodes and any legacy upgrade jobs.

![Upgrade status icon](upgrade-status-icon.png)
{scale="35%"}

### View the Status of Fleet Upgrades

The **Upgrade Status Tab** shows an overview of the Edge Nodes in this Fleet and
the upgrade status for each Node version. The Nodes are organized by version,
and the status columns display how many Nodes are in each status.

![A popup window titled "Fleet Upgrade Info" that shows the upgrade status of Edge Nodes in a Fleet](fleet-upgrade-status.png)

In this tab, you can view:

|Status|Description|
|------|-----------|
|<div style="width: 150px">Version</div>|Version for the Edge Nodes in this Fleet.|
|Total Nodes|Total number of Nodes in this Fleet running this version.|
|Upgrading|Nodes that are actively upgrading.|
|Skipped|Number of Nodes that were not upgraded due to one of these reasons: <ul><li>The Fleet [target version](#upgrading-edge-nodes) is not set.</li><li>No suitable upgrade package was found. You should use the [**Path** package upgrade](#configuring-edge-settings-for-path-upgrade) to add the specific upgrade package.</li><li>The Edge Node version is too old to receive an upgrade:<ul><li>Version 4.3.0 and older for Windows Nodes</li><li>Version 2.4.3 and older for Linux Nodes</li><li>CentOS 6.x Nodes</li></ul></li><li>The Leader couldn't download the upgrade package from the CDN.</li></ul>
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


### Legacy Upgrade Jobs

To view upgrades for Nodes older than 4.4.4, click the info icon in the **Target
Software Version** column and click the **Legacy Upgrade Jobs** tab.

All Nodes in the selected Fleet that are on version 4.4.4 or older will be
upgraded using the jobs framework. If legacy upgrade jobs exist, you can find
them here in the Legacy Job Upgrades tab.

