# Managing Destinations


For each [Destination](destinations) **type**, you can create multiple definitions,
each with different settings, to handle delivering data to different outputs with differing requirements.

Once a Destination definition is created, enabled and set up to receive data,
you can [verify](#verify-data-flow) that data is flowing correctly,
and [monitor](#monitor-destination-status) the Destination status to check for issues.

## Create a Destination

You can create a new Destination in two ways: by using [QuickConnect](#quickconnect) or [Routing](#routes).

For configuration settings specific to each Destination, check that Destination's documentation.

### Create a Destination with QuickConnect {#quickconnect}

To create a new Destination via [QuickConnect](quickconnect):

1. If you are on a Distributed deployment, first select **Fleets** in the sidebar and choose a Fleet.
1. Select **Collect**.
1. Select **Add Destination** at right, and select the Destination you want from the list.

For configuration settings specific to each Destination, check that Destination’s documentation.
After you've set up the Destination, you can [verify its data flow](#verify-data-flow).

### Create a Destination with Routes {#routes}

To create a new Destination via [Routing](routes):

1. If you are on a Distributed deployment, first select **Fleets** in the sidebar and choose a Fleet.
1. Select **More**, and then **Destinations**.
1. Select the Destination you want.
1. Select **Add Destination** to open a **New Destination** modal.

For configuration settings specific to each Destination, check that Destination’s documentation.
After you've set up the Destination, you can [verify its data flow](#verify-data-flow).

### Edit Destination Settings with JSON

To edit any Destination’s definition in a JSON text editor, select **Manage as JSON** at the bottom of the Destination modal.
You can directly edit multiple values, and you can use the **Import** and **Export** buttons
to copy and modify existing Destination configurations as `.json` files.

> When JSON configuration contains sensitive information, it is redacted during export.
>
{.box .info}

## Capture Outgoing Data

To capture data from a single enabled Destination, you can bypass the [Data Preview](data-preview) pane
and instead capture directly from the list of Destinations.
Select the **Live** button beside the Destination you want to capture.

![Screenshot of a Destination list with one default Destination and a Live button.](destination-live-button.eef2f9db2e.png)
{border="true" caption="Destination > Live button"}

You can also start an immediate capture from within an enabled Destination's config modal, by selecting the modal's **Live Data** tab.

![Screenshot of a Default Destination modal with the Live tab open and sample event.](destination-live-data-tab.b3ad974b73.png)
{border="true" caption="Destination modal > Live Data tab"}

## Verify Data Flow

Use the **Test** tab in the Destination's configuration modal to validate the configuration and connectivity of your Destination. A test sends sample events through the Destination to ensure it is correctly set up and can communicate with the downstream receiver.

You can choose from the available sample events or enter your own events, formatted as a JSON array.

## Monitor Destination Status

You can get a quick overview of Destination health status by referring to their status icons.

Additionally, each Destination's configuration modal offers two tabs for monitoring: [**Status**](#destination-status-tab) and [**Charts**](#destination-charts-tab).

### Destination Status Icons

Destination status icons are available on the **More** > **Destinations** page
and for each individual Destination in the list for a specific Destination type.

The icons have the following meanings:

| Icon | Meaning |
|----- | ------- |
| ![](status-icon-healthy.svg) | Healthy. Operating correctly. |
| ![](status-icon-warning.svg) | Warning. Experiencing issues.</br>The Destination is not functioning fully. Specific conditions will depend on the Destination type. |
| ![](status-icon-critical.svg)  | Critical. Experiencing critical issues.</br>Drill down to the Destination's [Status](#destination-status-tab) tab to find out the details. |
| ![](status-icon-disabled.svg)  | Disabled.</br> The Destination is configured, but not enabled. |
| ![](status-icon-configured.svg) | No health metrics available.</br>This may mean that a Destination is enabled, but has not been deployed yet. |
| | Inactive. When using [GitOps](stream/gitops), a Destination appears `Inactive` if its **Environment** field (configured under **Advanced Settings**) does not match the currently active environment determined by the deployed Git branch. This ensures integrations only activate in their designated environments, preventing unintended data flow or misconfiguration. |

### Destination Status Tab

The **Status** tab provides details about the Edge Nodes in the Fleet and their status.
An icon next to each Edge Node uses color to clearly signal its health:

- ![Green checkmark icon](status-icon-healthy.svg): All systems go! Your Edge Node is operating normally.
- ![Yellow warning sign icon](status-icon-warning.svg): Attention needed. There's a potential issue with this Edge Node.
- ![Red exclamation mark icon](status-icon-critical.svg): Stop! This Edge Node has encountered a critical error.

The way you view Edge Node statuses depends on the size of your Fleet:

- **Fewer than 1000 Edge Nodes**: All Edge Nodes are conveniently displayed on the Status tab, along with their statuses. Use the **Status** checkboxes at the top to filter the list by health (healthy, warning, error).

  Clicking on any Edge Node row dives deeper, providing specific details to help diagnose issues. The specific set of information provided depends on the Destination type. Keep in mind that this data only reflects process `0` for each Edge Node.

- **More than 1000 Edge Nodes**: With a larger Fleet, you can select a specific Edge Node from the drop-down menu (showing up to 100 Nodes). You can search by `hostname` or `GUID` to find a specific Node. You can also use the **Status** checkboxes to filter which Edge Nodes appear in the drop-down list.

> The content of the **Status** tab is loaded live when you open it and only displayed when all the data is ready.
> With a lot of busy Edge Nodes in a group, or Nodes located far from the Leader, there may be a delay before you see any information.
>
> The statistics presented are reset when the Edge Node restarts.
{.box .info}

### Destination Charts Tab

The **Charts** tab presents a visualization of the most recent 10 minutes of activity on the Destination. The following data is available:

- Total events out
- Average throughput (events per second)
- Total events dropped
- Total bytes out
- Average throughput (bytes per second)
- Backpressure

> This data (in contrast with the [status tab](#destination-status-tab)) is read almost instantly
> and does not reset when restarting an Edge Node.
{.box .info}
