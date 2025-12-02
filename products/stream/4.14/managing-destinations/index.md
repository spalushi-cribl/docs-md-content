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

1. If you are on a Distributed deployment, first select **Worker Groups** in the sidebar and choose a Worker Group.
1. Select **Routing**, then **QuickConnect**.
1. Select **Add Destination** at right, and select the Destination you want from the list.

For configuration settings specific to each Destination, check that Destination’s documentation.
After you've set up the Destination, you can [verify its data flow](#verify-data-flow).

### Create a Destination with Routes {#routes}

To create a new Destination via [Routing](routes):

1. If you are on a Distributed deployment, first select **Worker Groups** in the sidebar and choose a Worker Group.
1. Select **Data**, and then **Destinations**.
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

Destination status icons are available on the **Data** > **Destinations** page
and for each individual Destination in the list for a specific Destination type.

The icons have the following meanings:

| Icon | Meaning |
|----- | ------- |
| ![](status-icon-healthy.svg) | Healthy. Operating correctly. |
| ![](status-icon-warning.svg) | Warning. Experiencing issues.</br>The Destination is not functioning fully. Specific conditions will depend on the Destination type. |
| ![](status-icon-critical.svg)  | Critical. Experiencing critical issues.</br>Drill down to the Destination's [Status](#destination-status-tab) tab to find out the details. |
| ![](status-icon-disabled.svg)  | Disabled.</br> The Destination is configured, but not enabled. |
| ![](status-icon-configured.svg) | No health metrics available.</br>This may mean that a Destination is enabled, but has not been deployed yet. |
| | Inactive. When using [GitOps](gitops), a Destination appears `Inactive` if its **Environment** field (configured under **Advanced Settings**) does not match the currently active environment determined by the deployed Git branch. This ensures integrations only activate in their designated environments, preventing unintended data flow or misconfiguration. |

> You can also find status icons on the Cribl Stream front page on Worker Group tiles.
> ![Warning icon](status-icon-warning.svg) or ![Critical icon](status-icon-critical.svg) on any tile
> indicate that one or more Sources or Destinations in this Worker Group are experiencing issues.
> Hover over the icon to see more details.
{.box .success}

### Destination Status Tab

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

### Destination Charts Tab

The **Charts** tab presents a visualization of the most recent 10 minutes of activity on the Destination utlizing [internal metrics](internal-metrics). This data (in contrast with the [status tab](#destination-status-tab)) is read almost instantly and does not reset when restarting a Worker.

| Data | Metric | 
| --- | --- | 
| **Total events out** | `total.out_events` |
| **Average throughput (events per second)** | Derived from `total.out_events` |
| **Total events dropped** | `total.dropped_events` |
| **Total bytes out** | `total.out_bytes` |
| **Average throughput (bytes per second)** | Derived from `total.out_bytes` |
| **Backpressure** | `backpressure.outputs` and `blocked.outputs` |

The following charts are available on Cribl.Cloud deployments utilizing the `iometrics` [integration metrics](internal-metrics#integration), requiring a [metrics level](internal-metrics#integration-metrics-levels).

| Data | Metric | Level Required | 
| --- | --- | --- |
| **Endpoint health** | Averages the percentage of healthy endpoints from `endpoints_healthy_percentage`. | `Minimal` |
| **P95 latency** | `p95_duration_millis` – 95th percentile request latency in milliseconds. | `Detailed` |
| **P99 latency** | `p99_duration_millis` – 99th percentile request latency in milliseconds. | `Detailed` |
| **Failure rate** | Percentage of failed requests, calculated as `(failed_requests / total_requests) * 100`. | `Basic` |
