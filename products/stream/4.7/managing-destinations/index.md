# Managing Destinations


For each Destination **type**, you can create multiple definitions, depending on your requirements. 

## Create a Destination

To create a new Destination:

- In single-instance deployments, select **Manage**, then proceed to the options below.
- In distributed deployments, first select **Manage**,
  then a **Worker Group** to configure and choose one of the options below. 

You can configure a new Destination in two ways:

- To configure via the graphical [QuickConnect](quickconnect) UI:
    1. Select **Routing**, then **QuickConnect**.
    1. Select **Add Destination** at right.
    1. From the resulting drawer's tiles, select the desired Destination type and then **Add Destination**.
- Or, to configure via the [Routing](routes) UI:
    1. Select **Data**, then **Destinations**.
    1. From the resulting page's tiles or the **Destinations** left nav, select the desired type.
    1. Select **Add Destination** to open a **New Destination** modal.

For configuration settings specific to each Destination, check that Destination's documentation.

### JSON Destination Definition

To edit any Destination’s definition in a JSON text editor, select **Manage as JSON** at the bottom of Destination modal.
You can directly edit multiple values, and you can use the **Import** and **Export** buttons
to copy and modify existing Destination configurations as `.json` files.

> When JSON configuration contains sensitive information, it is redacted during export.
> 
{.box .info}

## Capture Outgoing Data

To capture data from a single enabled Destination, you can bypass the [Preview](data-preview) pane
and instead capture directly from a **Manage Destinations** page.
Select the **Live** button beside the Destination you want to capture.

![Screenshot of a Destination list with one default Destination and a Live button.](destination-live-button.eef2f9db2e.png)
{border="true" caption="Destination > Live button"}

You can also start an immediate capture from within an enabled Destination's config modal, by selecting the modal's **Live Data** tab. 

![Screenshot of a Default Destination modal with the Live tab open and sample event.](destination-live-data-tab.b3ad974b73.png)
{border="true" caption="Destination modal > Live Data tab"}

## Verifying Data Flow

Use the **Test** tab in the Destination's configuration modal to validate the configuration and connectivity of your Destination. A test sends sample events through the Destination to ensure it is correctly set up and can communicate with the downstream receiver.

You can choose from the available sample events or enter your own events, formatted as a JSON array.

## Monitor Destination Status

You can get a quick overview of Destination health status by referring to their status icons.

Additionally, each Destination's configuration modal offers two tabs for monitoring: [**Status**](#status-tab) and [**Charts**](#charts-tab).

### Destination Status Icons

Destination status icons are available on the **Data** > **Destinations** page
and for each individual Destination in the list for a specific Destination type.

The icons have the following meanings:

| Icon | Meaning |
|----- | ------- |
| ![](status-icon-healthy.svg) | Healthy. Operating correctly. |
| ![](status-icon-warning.svg) | Warning. Experiencing issues.</br>The Destination is not functioning fully. Specific conditions will depend on the Destination type. |
| ![](status-icon-critical.svg)  | Critical. Experiencing critical issues.</br>Drill down to the Destination's [Status](#status-tab) tab to find out the details. |
| ![](status-icon-disabled.svg)  | Disabled.</br> The Destination is configured, but not enabled. |
| ![](status-icon-configured.svg) | No health metrics available.</br>This may mean that a Destination is enabled, but has not been deployed yet. |
| | Inactive. When using [GitOps](gitops), a Destination appears `Inactive` if its **Environment** field (configured under **Advanced Settings**) does not match the currently active environment determined by the deployed Git branch. This ensures integrations only activate in their designated environments, preventing unintended data flow or misconfiguration. |

> You can also find status icons on the Cribl Stream front page on Worker Group tiles.
> ![Warning icon](status-icon-warning.svg) or ![Critical icon](status-icon-critical.svg) on any tile
> indicate that one or more Sources or Destinations in this Worker Group are experiencing issues.
> Hover over the icon to see more details.
{.box .success}

### Status Tab

The **Status** tab provides details about the Workers in the group and their status.
An icon shows whether the Worker is operating normally.

You can select each Worker's row to see specific information,
for example, to identify issues when the Destination displays an error.
The specific set of information provided depends on the Destination type.
The data represents only process 0 for each Worker Node.

> The content of the **Status** tab is loaded live when you open it and only displayed when all the data is ready.
> With a lot of busy Workers in a group, or Workers located far from the Leader, there may be a delay before you see any information.
> 
> The statistics presented are reset when the Worker restarts.
{.box .info}

### Charts Tab

The **Charts** tab presents a visualization of the recent activity on the Destination.
The following data is available:

- Events in
- Thruput in (events per second)
- Bytes in
- Thruput in (bytes per second)
- Blocked status

> This data (in contrast with the [status tab](#status-tab)) is read almost instantly
> and does not reset when restarting a Worker.
{.box .info}
